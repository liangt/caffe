# Multilabel Caffe

[![License](https://img.shields.io/badge/license-BSD-blue.svg)](LICENSE)

This is a modified Caffe which is convenient for me to do multilabel classification. Although, there are many ways to do multilabel classification in Caffe, such as using two Data layer, one for image and the other for label, I'd like to have a more intuitive way to do it.

Here are changes I make:
* MultilabelData layer: similar to Data layer expect that MultilabelData use multilabel_data_param instead of data_param.
* MultilabelDataParameter: similar to DataParameter expect that MultilabelDataParameter has an extra attribute label_size which is the total number of labels.
* MultilabelDatum: similar to Datum expect that the label in MultilabelDatum is repeated.
* tools/convert_multilabel_imageset: convert a set of images to a lmdb/leveldb by storing them as MultilabelDatum proto buffers.
 
      convert_multilabel_imageset [FLAGS] filelist db_name
   The **filelist** is a list of files as well as their labels, in the format as:
      
      img_path label_1,label_2,label_3,...
   label_i is an integer in \[0, label_size).
   
* add a function to output the label probability for each test sample.

An example of MultilabelData layer:
```
layer {
  name: "data"
  type: "MultilabelData"
  include {
    phase: TRAIN
  }
  top: "data"
  top: "label"
  transform_param {
    crop_size: 224
    mean_value: 104
    mean_value: 117
    mean_value: 123
    mirror: true
  }
  multilabel_data_param {
    source: "image_db"
    batch_size: 8
    label_size: 66
    backend: LMDB
  }
}
```

# Caffe

[![Build Status](https://travis-ci.org/BVLC/caffe.svg?branch=master)](https://travis-ci.org/BVLC/caffe)
[![License](https://img.shields.io/badge/license-BSD-blue.svg)](LICENSE)

Caffe is a deep learning framework made with expression, speed, and modularity in mind.
It is developed by the Berkeley Vision and Learning Center ([BVLC](http://bvlc.eecs.berkeley.edu)) and community contributors.

Check out the [project site](http://caffe.berkeleyvision.org) for all the details like

- [DIY Deep Learning for Vision with Caffe](https://docs.google.com/presentation/d/1UeKXVgRvvxg9OUdh_UiC5G71UMscNPlvArsWER41PsU/edit#slide=id.p)
- [Tutorial Documentation](http://caffe.berkeleyvision.org/tutorial/)
- [BVLC reference models](http://caffe.berkeleyvision.org/model_zoo.html) and the [community model zoo](https://github.com/BVLC/caffe/wiki/Model-Zoo)
- [Installation instructions](http://caffe.berkeleyvision.org/installation.html)

and step-by-step examples.

[![Join the chat at https://gitter.im/BVLC/caffe](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/BVLC/caffe?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Please join the [caffe-users group](https://groups.google.com/forum/#!forum/caffe-users) or [gitter chat](https://gitter.im/BVLC/caffe) to ask questions and talk about methods and models.
Framework development discussions and thorough bug reports are collected on [Issues](https://github.com/BVLC/caffe/issues).

Happy brewing!

## License and Citation

Caffe is released under the [BSD 2-Clause license](https://github.com/BVLC/caffe/blob/master/LICENSE).
The BVLC reference models are released for unrestricted use.

Please cite Caffe in your publications if it helps your research:

    @article{jia2014caffe,
      Author = {Jia, Yangqing and Shelhamer, Evan and Donahue, Jeff and Karayev, Sergey and Long, Jonathan and Girshick, Ross and Guadarrama, Sergio and Darrell, Trevor},
      Journal = {arXiv preprint arXiv:1408.5093},
      Title = {Caffe: Convolutional Architecture for Fast Feature Embedding},
      Year = {2014}
    }
