Project Title: AWS Image Recognition Pipeline

Description/Problem Statement: Developed an image recognition pipeline on AWS using a combination of EC2 instances, S3, SQS, and Rekognition services. The objective was to perform object detection and text recognition on images stored in an S3 bucket, utilizing Java applications running on Amazon Linux VMs.

Skills Utilized:
- AWS (EC2, S3, SQS, Rekognition)
- Java Programming
- Image Processing
- Parallel Computing
- Queue Management

Solution:
- Utilized two EC2 instances (EC2 A and B) running Amazon Linux AMI to operate in parallel.
- EC2 A retrieved 10 images from the designated S3 bucket (njit-cs-643) and conducted object detection using Rekognition.
- Identified images with cars detected with a confidence level exceeding 90%, storing their indexes in SQS.
- EC2 B continuously monitored SQS for image indexes, downloading images from S3 and performing text recognition using Rekognition.
- The instances operated independently and concurrently, ensuring efficient processing of image data.
- Implemented a signaling mechanism using a special index (-1) in the SQS queue to indicate the completion of image processing by EC2 A.

Program Output:
Upon completion, EC2 B generated an output file, "output.txt," on its associated EBS. This file contained:
- Indexes of images featuring both cars and text.
- The corresponding text identified within each image.

The application was designed to function seamlessly regardless of which instance initiated processing first.
