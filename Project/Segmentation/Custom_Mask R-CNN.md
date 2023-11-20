**cf**[^0]

1.  *git clone https://github.com/soumyaiitkgp/Custom_MaskRCNN.git
2. Anaconda Virtual Environment 실행
	- *conda activate (가상환경 이름)* 
3. ==GPU 환경에 맞게 해당 패키지 버전 설치==
	- _nvidia-smi_
	- 이 과정이 제일 중요 
	- _pip install tensorflow=2.7.0_
4. _pip3 install scipy Pillow cython matplotlib scikit-image opencv-python h5py imgaug IPython[all]_
5. _python3 setup.py install_
6. _python custom.py train — dataset=D:/../CustomMask_RCNN/samples/custom/dataset — weights=coco_


#### 오류

1. python custom.py train — dataset=/home/ohanthony/test/Custom_MaskRCNN/samples/custom/dataset — weights=coco 실행 시 

















[^0]: https://medium.com/analytics-vidhya/a-simple-guide-to-maskrcnn-custom-dataset-implementation-27f7eab381f2
	- https://github.com/soumyaiitkgp/Custom_MaskRCNN?source=post_page-----27f7eab381f2--------------------------------
