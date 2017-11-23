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
