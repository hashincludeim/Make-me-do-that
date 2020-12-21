# Make-me-do-that
A tool that can be used to transfer the pose and attire from a source subject to a target subject. Implemented using GANs.

# Generator
The generator takes a noise vector of the latent dimension and generates an image.
The shape of the image should be the same as the shape of the discriminator's input.
The generator first upsamples the noise vector with the dense layer, in order to have
enough values to reshape into the first generator block. The goal of the projection
is to have the same dimension as the last block in the discriminator architecture.
This is equivalent to 4 x 4 x number of filters in the last convolutional layer of the
discriminator. Each generator block applies deconvolution to upsample the image
and batch normalization. We use 4 decoder blocks and a final convolution layer to
get a 3D tensor, which represents a fake image that has 3 channels.

# Discriminator
The discriminator is an image classifier. We use a convolutional neural network
instead, with 4 blocks of layers. Each block includes a convolution, batch normal-
ization and another convolution with striding to downscale the image by a factor
two and another batch normalization. The result goes through average pooling,
followed by a dense sigmoid layer which returns a single output probability.

# Training
1. Select a number of real images from the training set.
2. Generate a number of fake images. This is done by sampling random noise
vectors and creating images from them using the generator.
3. Train the discriminator for one or more epochs using both fake and real images.
This will update only the discriminator's weights by labeling all the real images
as 1 and the fake images as 0.
4. Generate another number of fake images.
5. Train the full GAN model for one or more epochs using only fake images. This
will update only the generator's weights by labeling all fake images as 1.

# Transfer
1. The fully trained Generator is fed with the normalized pose.
2. The generator produces the image.
3. The image then undergoes warping and texturing of clothes.
4. Resultant image is produced.
