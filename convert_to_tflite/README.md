Copy model.ckpt and graph.pb to here.

Find ${tensorflow_dir}/lib/python3.5/site-packages/tensorflow/python/tools/freeze_graph.py.

Find ${tensorflow_dir}/lib/python3.5/site-packages/tensorflow/python/tools/optimize_for_inference.py.

Then:

## freeze:

```
python3 freeze_graph.py \
--input_graph=graph.pb \
--input_checkpoint=model.ckpt \
--input_binary=true \
--output_graph=frozen_graph.pb \
--output_node_names=SRGAN_g/out/Tanh
```
 
## optimize:

```
python3 optimize_for_inference.py \
--input=frozen_graph.pb \
--output=optimized_frozen_graph.pb \
--frozen_graph=True \
--input_names=input_image \
--output_names=SRGAN_g/out/Tanh \
--alsologtostderr
```

## convert to tflite:

```
toco \
--graph_def_file=optimized_frozen_graph.pb \
--output_file=srgan_224_4.tflite \
--input_format=TENSORFLOW_GRAPHDEF \
--output_format=TFLITE \
--input_shapes=1,224,224,3 \
--input_arrays=input_image \
--output_arrays=SRGAN_g/out/Tanh \
--inference_type=FLOAT \
--logtostderr
```

P.S.: linux only