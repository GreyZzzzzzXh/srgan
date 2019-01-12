Copy ***model.ckpt*** and ***graph.pb*** here.


Then:

## freeze:

```
python3 -m tensorflow.python.tools.freeze_graph \
--input_graph=graph.pb \
--input_checkpoint=model.ckpt \
--input_binary=true \
--output_graph=frozen_graph.pb \
--output_node_names=SRGAN_g/out/Tanh
```

## convert to tflite:

```
toco \
--input_file=frozen_graph.pb \
--output_file=srgan_224_4.tflite \
--input_format=TENSORFLOW_GRAPHDEF \
--output_format=TFLITE \
--input_shapes=1,224,224,3 \
--input_arrays=input_image \
--output_arrays=SRGAN_g/out/Tanh \
--inference_type=FLOAT \
--logtostderr
```

env:

Linux Ubuntu 16.04 / Python 3.5 / TensorFlow 1.7
