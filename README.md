Access the files uploaded via the link https://drive.google.com/drive/folders/1jsze18IMLJRui8jZ-Szdh5ITfMSeKa_d?usp=sharing

# Real-Time DNN Deployment for Laser Sensing

This repository presents our research on deploying binarized deep neural networks (BNNs) for laser fringe pattern detection which is a core problem in optical sensing and interferometry. Instead of relying on bulky GPUs or offline image processing, our project explores how deep learning models can run efficiently on FPGA-like platforms for real-time, low-power fringe classification.

## Project Motivation

Laser fringe analysis is central to many optical and metrology systems from structural monitoring to vibration sensing. Traditionally, these systems depend on Fourier or wavelet-based algorithms, which are powerful but computationally expensive and hard to scale to real-time embedded applications.

We wanted to change that.
The question we asked was simple:

“Can we make neural networks so light that they can classify laser fringes in real time even on an FPGA?”

This led us to experiment with binarization techniques, where neural networks use only +1 and –1 weights and activations, drastically reducing memory and computation.

## Research Objective

Our goal was to design a compact deep learning model that:

Classifies positive and negative laser fringe patterns in real time.
Uses XNOR-based binary convolutions instead of floating-point arithmetic.
Operates efficiently under strict hardware memory and latency constraints.
Bridges the gap between software-trained CNNs and hardware-compatible inference logic.

Core Approach

We began by testing XNOR-Net binarization on smaller architectures like AlexNet using the CIFAR-10 dataset.
Once the technique proved functional, we scaled it to object detection models (YOLOv1 and YOLOv5) to study how binarization behaves in complex, real-world architectures.

Our research evolved in three phases:

## 1. AlexNet — Proof of Concept

We implemented XNOR-style binary convolutions in AlexNet and verified that classification could still be achieved with reduced precision.
Results confirmed significant savings in computation with acceptable accuracy trade-offs.

## 2. YOLOv1 — Early Object Detection

We extended binarization to YOLOv1, which performed dense detection using grid-based classification.
However, due to its shallow architecture and reliance on high-resolution feature maps, binarization degraded spatial precision, showing the first limitations of pure BNNs.

## 3. YOLOv5 — Advanced Testing

Finally, we attempted binarization on YOLOv5n, the lightweight variant of the YOLO family.
This was the most challenging phase: YOLOv5’s C3 (CSP bottleneck) structure depends on skip connections and partial gradient flow mechanisms that don’t play well with binary activations.
The binarized model struggled to converge, exposing the trade-offs between hardware efficiency and learning stability.
Despite these limitations, the project demonstrated how binarization can drastically simplify neural networks for deployment, especially when adapted carefully to hardware constraints.

## System Design Overview

Our design pipeline included:

# Stage                                                            	Description
Dataset & Preprocessing	                                            Synthetic laser fringe images generated in MATLAB and normalized to 320×320 grayscale.
Model Adaptation	                                                   YOLOv5 modified for binary (+1/−1) weights and activations using XNOR-Net principles.
Quantization & Static Memory Design                               	 Removed dynamic memory allocation and restructured dataflow for HLS/FPGA compatibility.
Simulation & Verification	                                          Outputs validated against MATLAB-generated reference data and PyTorch baselines.
Key Insights

Binarization offers a massive reduction in compute and memory footprint, enabling real-time inference potential on FPGAs.
However, complex architectures like YOLOv5 require structural adaptations to remain trainable under binary constraints.
The combination of YOLO’s feature extraction power with XNOR-Net’s hardware efficiency is promising for future edge AI systems.

# Results                                                           Summary

# Model           Type                     Dataset                                   Outcome

 AlexNet	        Binarized	               CIFAR-10	                   Stable training, functional accuracy
 YOLOv1           Binarized	               Fringe dataset              Detected fringes, low IoU
 YOLOv5n	        Partial binarized	       Fringe dataset	             Training unstable due to CSP structure
 Full-Precision 	Baseline	               Fringe dataset	             High accuracy and consistent classification

Despite reduced accuracy in the binarized versions, the project validated the feasibility of lightweight binary architectures for optical pattern classification.

Concluding Remarks

This project bridges deep learning and embedded hardware design, showing that efficient neural inference is possible without relying on large compute platforms.
Although full FPGA deployment was not completed, the groundwork from algorithm selection and model binarization to hardware synthesis design lays the foundation for future real-time implementations.


