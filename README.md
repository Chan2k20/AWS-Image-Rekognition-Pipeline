# AWS-Image-Rekognition-Pipeline


Built an image recognition pipeline in AWS, using two EC2 instances, S3, SQS,
and Rekognition. The assignment must be done in Java on Amazon Linux VMs. For the rest of the
description, you should refer to the figure below:



Created 2 EC2 instances (EC2 A and B in the figure), with Amazon Linux AMI, that will work
in parallel. Each instance will run a Java application. Instance A will read 10 images from an S3 bucket that
we created (njit-cs-643) and perform object detection in the images. When a car is detected using
Rekognition, with confidence higher than 90%, the index of that image (e.g., 2.jpg) is stored in SQS.

Instance B reads indexes of images from SQS as soon as these indexes become available in the queue, and
performs text recognition on these images (i.e., downloads them from S3 one by one and uses Rekognition
for text recognition). 

Note that the two instances work in parallel: for example, instance A is processing
image 3, while instance B is processing image 1 that was recognized as a car by instance A. When instance
A terminates its image processing, it adds index -1 to the queue to signal to instance B that no more indexes
will come. 

Program output: When instance B finishes, it prints to a file output.txt, in its associated EBS,
the indexes of the images that have both cars and text, and also prints the actual text in each image next to
its index. Your application must work no matter which instance starts first.
