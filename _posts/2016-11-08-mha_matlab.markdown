---
layout:     post
title:      "Read and write .mha file in Matlab(medical imaging)"
subtitle:   ""
date:       2016-11-08 12:47:51
author:     "Pingge Jiang"
header-img: "img/home-bg.jpg"
catalog:    true
categories: research
tags:
    - [coding, matlab, mha]

---
These two Matlab functions read and write .mha medical images.

The structure supported:

<br /> * NDims
<br /> * BinaryData
<br /> * BinaryDataByteOrderMSB
<br /> * ElementNumberOfChannels
<br /> * ElementType
<br /> * ElementSpacing
<br /> * Offset
<br /> * ElementDataFile

(c++ version also available in this site)

## readmha.m
```matlab
function [A,Ainfo] = readmha(fn)
%% Usage: [A,Ainfo] = readmha(fn)

fp = fopen(fn,'rb');
if (fp == -1)
  error ('Cannot open mha file for reading');
end

%% Parse header
dims = 3;
binary = 1;
binary_msb = 0;
nchannels = 1;
Ainfo = [];
for i=1:20
  t = fgetl(fp);

  [a,cnt] = sscanf(t,'NDims = %d',1);
  if (cnt > 0)
    dims = a;
    continue;
  end

  [a,cnt] = sscanf(t,'BinaryData = %s',1);
  if (cnt > 0)
    if (strcmpi(a,'true'))
      binary = 1;
    else
      binary = 0;
    end
    continue;
  end

  [a,cnt] = sscanf(t,'BinaryDataByteOrderMSB = %s',1);
  if (cnt > 0)
    if (strcmpi(a,'true'))
      binary_msb = 1;
    else
      binary_msb = 0;
    end
    continue;
  end

  %% [a,cnt,errmsg,ni] = sscanf(t,'DimSize = ',1);
  ni = strfind(t,'DimSize = ');
  if (~isempty(ni))
    ni = ni + length('DimSize = ');
    [b,cnt] = sscanf(t(ni:end),'%d');
    if (cnt == dims)
      sz = b;
      continue;
    end
  end

  [a,cnt] = sscanf(t,'ElementNumberOfChannels = %d',1);
  if (cnt > 0)
    nchannels = a;
    continue;
  end

  [a,cnt] = sscanf(t,'ElementType = %s',1);
  if (cnt > 0)
    element_type = a;
    continue;
  end

  %% [a,cnt,errmsg,ni] = sscanf(t,'ElementSpacing = ',1);
  ni = strfind(t,'ElementSpacing = ');
  if (~isempty(ni))
    ni = ni + length('ElementSpacing = ');
    [b,cnt] = sscanf(t(ni:end),'%g');
    if (cnt == dims)
      Ainfo.ElementSpacing = b;
      continue;
    end
  end
  
  %% [a,cnt,errmsg,ni] = sscanf(t,'Offset = ',1);
  ni = strfind(t,'Offset = ');
  if (~isempty(ni))
    ni = ni + length('Offset = ');
    [b,cnt] = sscanf(t(ni:end),'%g');
    if (cnt == dims)
      Ainfo.Offset = b;
      continue;
    end
  end
  
  [a,cnt] = sscanf(t,'ElementDataFile = %s',1);
  if (cnt > 0)
    data_loc = a;
    break;
  end
end

if (strcmp(element_type,'MET_FLOAT'))
  [A,count] = fread(fp,sz(1)*sz(2)*sz(3)*nchannels,'float');
elseif (strcmp(element_type,'MET_UCHAR'))
  [A,count] = fread(fp,sz(1)*sz(2)*sz(3)*nchannels,'uchar');
elseif (strcmp(element_type,'MET_UINT'))
  [A,count] = fread(fp,sz(1)*sz(2)*sz(3)*nchannels,'uint32');
else
  [A,count] = fread(fp,sz(1)*sz(2)*sz(3)*nchannels,'int16');
end
%% A = reshape(A,sz(1),sz(2),sz(3),nchannels);
A = reshape(A,nchannels,sz(1),sz(2),sz(3));
A = shiftdim(A,1);

fclose(fp);

return;
```

## writemha.m:

```matlab
function writemha(fn,A,offset,spacing,type)
%% writemha(fn,A,offset,spacing,type)
%%   fn        filename
%%   A         Volume
%%   offset
%%   spacing
%%   type      'uchar','float', or 'short'

Asz = size(A);

switch ndims(A)
 case 3,
 case 4,
  if (size(A,4) ~= 3)
    error ('Sorry, a 4D volume must be a vector field');
  end
 otherwise
  error ('Sorry, only 3D & VF volumes supported');
end

fp = fopen(fn,'w');
if (fp == -1)
  error ('Cannot open mha file for writing');
end

fprintf (fp,'ObjectType = Image\n');
fprintf (fp,'NDims = 3\n');
fprintf (fp,'BinaryData = True\n');
fprintf (fp,'BinaryDataByteOrderMSB = False\n');
fprintf (fp,'Offset = ');
fprintf (fp,' %g',offset);
fprintf (fp,'\n');
fprintf (fp,'ElementSpacing = ');
fprintf (fp,' %g',spacing);
fprintf (fp,'\n');
fprintf (fp,'DimSize = ');
fprintf (fp,' %d',Asz(1:3));
fprintf (fp,'\n');
fprintf (fp,'AnatomicalOrientation = RAI\n');
if (ndims(A) == 4)
  fprintf(fp,'ElementNumberOfChannels = 3\n');
end
fprintf (fp,'TransformMatrix = 1 0 0 0 1 0 0 0 1\n');
fprintf (fp,'CenterOfRotation = 0 0 0\n');

%% This works for 3D (A stays the same), and 4D (shift left 1)
A = shiftdim(A,3);

switch(lower(type))
 case 'uchar'
  fprintf (fp,'ElementType = MET_UCHAR\n');
  fprintf (fp,'ElementDataFile = LOCAL\n');
  fwrite (fp,A,'uint8');
 case 'short'
  fprintf (fp,'ElementType = MET_SHORT\n');
  fprintf (fp,'ElementDataFile = LOCAL\n');
  fwrite (fp,A,'int16');
 case 'ushort'
  fprintf (fp,'ElementType = MET_USHORT\n');
  fprintf (fp,'ElementDataFile = LOCAL\n');
  fwrite (fp,A,'uint16');
 case 'uint32'
  fprintf (fp,'ElementType = MET_UINT\n');
  fprintf (fp,'ElementDataFile = LOCAL\n');
  fwrite (fp,A,'uint32');
 case 'float'
  fprintf (fp,'ElementType = MET_FLOAT\n');
  fprintf (fp,'ElementDataFile = LOCAL\n');
  fwrite (fp,A,'real*4');
 otherwise
  fclose(fp);
  error ('Sorry, unsupported type');
end

fclose(fp);
```
