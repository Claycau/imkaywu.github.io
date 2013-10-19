---
layout:post
title:A MATLAB implementation of distance transform algorithm
tag:[computer vision, image process, distance transform, MATLAB]
categories:blog
---

In the gesture recognition algorithms we are developing, I need to calculate the transform distance of a binary image. Though the MATLAB has a function for just that purpose, I decided to dig around and find more efficient ways to implement this algorithm.

For folks who are not familiar with distance transform, there is a great post[http://homepages.inf.ed.ac.uk/rbf/HIPR2/distance.htm] for you. This can give a big picture of what this task is about.

The algorithm I implement is using **chessboard** distance metric. And I'm still working on some other metrics. For instance, **Euclidean** and **city block** metrics.

First, we assume that the pixel value of the object is 1, and that of the background is 0. Then distance transform is defined as the smallest distance between the object and background pixels. And this value is used to replace the pixel value of the object.

##Logic of the algorithm

direction: up-left to bottom-right

for each pixel

	if the value is greater than 0 (namely, object pixel)

		get the pixels from the *left*, *up-left*, *up*, *up-right* directions

		add 1 to the minimum value, assign it to the object pixel

	end if

end for

direction: bottom-right to up-left

for each pixel

	if the value is greater than 0 (namely, object pixel)

		get the pixels from the *right*, *bottom-right*, *bottom*, *bottom-left* directions

		add 1 to the minimum value, assign it to the object pixel

	end if

end for

And the MATLAB code is as follows:

{% highlight MATLAB %}
function dt=dist_trans(mat)
    row=size(mat,1);
    colum=size(mat,2);
    index0=(mat==0);
    index1=(mat==1);
    mat(index0)=1;
    mat(index1)=0;
    
    for i=1:row
        for j=1:colum
            if(mat(i,j)>=1)
                if((i==1)&&(j==1));
                elseif(i==1)
                    mat(i,j)=min(mat(i,j-1))+1;
                elseif(j==1)
                    mat(i,j)=min([mat(i-1,j),mat(i-1,j+1)])+1;
                elseif(j==colum)
                    mat(i,j)=min([mat(i,j-1),mat(i-1,j-1),mat(i-1,j)])+1;
                else
                    mat(i,j)=min([mat(i,j-1),mat(i-1,j-1),mat(i-1,j),mat(i-1,j+1)])+1;
                end
            end
        end
    end
    
    for i=row:-1:1
        for j=colum:-1:1
            if(mat(i,j)>=1)
                if((i==row)&&(j==colum));
                elseif(i==row)
                    mat(i,j)=mat(i,j+1)+1;
                elseif(j==colum)
                    mat(i,j)=min([mat(i+1,j),mat(i+1,j-1)])+1;
                elseif(j==1)
                    mat(i,j)=min([mat(i,j+1),mat(i+1,j+1),mat(i+1,j)])+1;
                else
                    mat(i,j)=min([mat(i,j+1),mat(i+1,j+1),mat(i+1,j),mat(i+1,j-1)])+1;
                end
            end
        end
    end
    dt=mat;
end
{% endhightlight %}

The advantage of this algorithm is its simlicity and easy to understand. There are lots of research in improving this algorithm and I'm just started my own research. So I hope more sophisticated and faster algorithms can come up later.