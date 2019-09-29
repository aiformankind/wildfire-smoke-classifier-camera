# Wildfire Smoke Classifier


#### Quick Start

**Building Wildfire Smoke Classifier via Transfer Learning**

Install Docker:

https://docs.docker.com/v17.12/docker-for-mac/install/#install-and-run-docker-for-mac

We use the scripts provided by the following excellent Google’s TensorFlow For Poets codelab.
https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/#0

Increase the memory available to Docker Engine
1. Navigate to Docker Preferences
2. Select Advanced
3. Increase Memory to 4.0 GiB

Clone the repository (https://github.com/aiformankind/wildfire-smoke-classifier-camera.git):
```
git clone https://github.com/aiformankind/wildfire-smoke-classifier-camera.git
```

Go to the repository directory that you just clone:
```
cd wildfire-smoke-classifier-camera
```

Build the Tensorflow docker (this job will pull the latest tensorflow images and set up the environment) :
```
docker build -t aiformankind/wildfire-smoke-camera:0.0.1 .
```

Start the Tensorflow container (this job will spin up the seetheworld container):
```
docker run -it -p 8888:8888 -p 6006:6006 --name=wildfire-smoke-camera aiformankind/wildfire-smoke-camera:0.0.1
```

Augment data:
```
python augment/augment_images.py --image_dir=data/usa/wildfire_smoke --num_samples=3000
```

Retrain model:
```
python -m train.retrain   --bottleneck_dir=train_output/bottlenecks   --how_many_training_steps=500   --model_dir=train_output/models/   --summaries_dir=train_output/training_summaries/"${ARCHITECTURE}"   --output_graph=train_output/retrained_graph.pb   --output_labels=train_output/retrained_labels.txt   --architecture="${ARCHITECTURE}" --image_dir=augment-data/usa/wildfire_smoke
```

Predict label:
```
python -m train.label_image --graph=train_output/retrained_graph.pb --image=validation/usa/wildfire_smoke/00000001.jpg
```


#### Troubleshooting

* Problem: The **Augment data** step fails with error 'Killed'. Resolution: Increase memory allocated to Docker Engine and rerun.

