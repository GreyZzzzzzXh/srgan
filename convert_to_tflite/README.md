# Train and Convert to TfLite
Steps below show how to train and convert the model to tflite format.

Env: 
    Linux Ubuntu 16.04 / Python 3.5 / TensorFlow 1.7

## 1. modify SubpixelConv2d

SubpixelConv2d uses tf.depth_to_space(), which is not supported in tflite. 

Go to the definition of SubpixelConv2d() and replace **tf.depth_to_space()** with **tf.transpose()** and **tf.batch_to_space_nd()**, see https://github.com/GreyZzzzzzXh/srgan/blob/master/model.py#L55.

## 2. train and evaluate 
Train and evaluate model according to [srgan/README.md](https://github.com/GreyZzzzzzXh/srgan/blob/master/README.md).

Once finishing evaluate, you will get some new files in srgan/checkpoint.

## 3. freeze graph
You will get frozen_graph.pb.

    $ cd srgan/checkpoint
    $ python3 -m tensorflow.python.tools.freeze_graph \
    --input_graph=graph.pb \
    --input_checkpoint=model.ckpt \
    --input_binary=true \
    --output_graph=frozen_graph.pb \
    --output_node_names=SRGAN_g/out/Tanh

## 4. convert to tflite
You can change the input size.

    $ toco \
    --input_file=frozen_graph.pb \
    --output_file=srgan_96_4.tflite \
    --input_format=TENSORFLOW_GRAPHDEF \
    --output_format=TFLITE \
    --input_shapes=1,96,96,3 \
    --input_arrays=input_image \
    --output_arrays=SRGAN_g/out/Tanh \
    --inference_type=FLOAT \
    --logtostderr

Then you'll get a tflite model named srgan_96_4.tflite.


