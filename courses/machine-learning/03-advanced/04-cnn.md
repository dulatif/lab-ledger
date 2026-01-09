# Lesson 4: Computer Vision (CNN)

## Learning Goals

- Convolutions.
- Pooling.

## 1. The Problem with MLP for Images

Flattening a 1080p image results in 6 million inputs. Too many weights.
Spatial structure is lost.

## 2. The Convolution Operation

A small "Filter" (Kernel) slides over the image to detect features (Edges, Corners, Textures).
Weight sharing reduces parameter count drastically.

## 3. Pooling

Downsampling the image (Max Pooling) to reduce dimensions and make the model invariant to small translations.

## Key Takeaways

- CNNs learn hierarchy: Edges -> Shapes -> Objects.
- ResNet is the standard backbone architecture.
