# DATA-FOLDING
LOSSLESS COMPRESSION

IN THIS PROJECT WE FOLD DATA USING A PROPERTY CALLED ADJACENT NEIGHBOR REDUNDANCY
WHERE COLUMN FOLDING FOLLOWED BY ROW FOLDING IS DONE.





%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
 
% NAME: SNEHA V BHAMBURE
% RED ID- 820927894
 
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
clc;
clear all;
Im = imread('scene3.jpg');
Im = im2double(rgb2ycbcr(Im));
Im = Im(:,:,1);
[row1,column1,N] = size(Im);
 
if row1 > column1    
    rgb = padarray(Im,[row1-column1 0],'pre'); % adds zero if row>column to equalize the dimensions
elseif row1 < column1    
    rgb = padarray(Im,[column1-row1 0],'pre');  % adds zero if row<column to equalize the dimensions
else
     rgb = Im;
end;
 
 
figure
imshow(rgb);
title('original image');
for i = 1:4
[row1,column1,N] = size(rgb)
 
%column folding
 for i = 1:row1
    for j = 1:column1/2
    f(i,j) = rgb(i,(2*j)-1) - rgb(i,2*j);%substracts the odd columns from even columns and then stores it in buffer f
    k(i,j) = rgb(i,(2*j)-1);% odd columns are stored in buffer k and then it is given as input to row folding 
    end;
  end;
 y = column1/2; % used to calculate the location in buffer f while performing row foldimg. 
[row2,column2] = size(k); % new size of buffer k.
 
 %row folding
 for i = 1:row2/2 
    for j = 1:column2
       f(i,y+j) = k((2*i)-1,j) - k(2*i,j); %  substract odd rows from even rows and store it in buffer f
    end;
  end;
 
 %remaining matrix
 for i = 1:row1/2
    for j = 1:column1/2
        x = row1/2; %used to calculate the location in buffer f
    f(x+i,y+j) = rgb((2*i)-1,(2*j)-1); % odd values from original image are stored in remaining empty buffer f. 
    end;
 end;
 rgb = f;
 figure, 
imshow(rgb);
end
%%
%Data Unfolding
[row3,column3,r] = size(rgb); 
 
for i = 1:4 
    %row unfolding
for i = 1:row3
    for j = 1:column3/2
        v(i,j) = rgb(i,column3/2+j);% copying the right half submatrix of rgb into buffer v
    end;
end;
size(v)
 
for i = 1:row3/2
    for j = 1:column3/2
        w((2*i-1),j) = v(row3/2+i,j);% calculating odd rows in buffer w
    end;
end;
size(w)
 
 
for i = 1:row3/2
    for j = 1:column3/2
        w(2*i,j) = v(row3/2+i,j)-v(0+i,j);% calculating even rows in buffer w
    end;
end;
 
size(w)
for i = 1:row3
    for j = 1:column3/2
        rgb(i,column3/2+j) = w(i,j); %input to column unfolding by replacing right half columns of rgb by buffer w
    end;
end;
 
%column unfolding
 
for i = 1:row3
    for j = 1:column3/2
        unfolded(i,(2*j)-1) = rgb(i,column3/2+j);% calculating odd columns of buffer unfolded
    end;
end;
 
for i = 1:row3
    for j = 1:column3/2
        unfolded(i,2*j) = rgb(i,column3/2+j)-rgb(i,j);% calculating even columns of buffer unfolded
    end;
end;
figure,
imshow(unfolded)
rgb = unfolded;
end;



%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

Image whose path is to be given for the code in MATLAB

 

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
