# Train faster rcnn on my own dataset

I refer [here](http://blog.csdn.net/sinat_30071459/article/details/51332084) for details.

## Methods

At first, you should label your data in the VOC format follow [here](https://github.com/rbgirshick/py-faster-rcnn#beyond-the-demo-installation-for-training-and-testing-models). Then rename `VOCdevkit` to `VOCdevkit2007`.

## Problems and solutions

1. **KeyError: 'max_overlaps'**
   
   If you have meet the problems in the similiar format, you can try to delete the `<py-faster-rcnn-root>/data/cache` and `<py-faster-rcnn-root>/data/VOCdevkit2007/annotations_cache`.
   
2. **KeyError: 'ColaCanBlack'**

   It caused by that your labels contain upper characters. Try to modify the `pascal_voc.py` in line 211:
   
   ```python
   # cls = self._class_to_ind[obj.find('name').text.lower().strip()]
   cls = self._class_to_ind[obj.find('name').text.strip()]
   ```
   
   If this is not work, refer [here](http://blog.csdn.net/sinat_30071459/article/details/51694037) for datails.
   
3. **Use your own dataset folder**

   There are two ways to train on your own datasets. You can replace the items under `Annotations, ImagesSets, JPEGImages` by your file in `<py-faster-rcnn-root>/data/VOCdevkit2007/VOC2007/` folder.
   
   The another way is modidy the code to change path. There are three position need to be modified. ('bottle' is my dataset folder.)
   
  pascal_voc.py : __init__ function:
   
   ```python
   # self._data_path = os.path.join(self._devkit_path, 'VOC' + self._year)
   self._data_path = os.path.join(self._devkit_path, 'bottle')
   ```
   
   pascal_voc.py : _do_python_eval function:
   
   ```
   annopath = os.path.join(
            self._devkit_path,
            # 'VOC' + self._year,
            'bottle',
            'Annotations',
            '{}.xml')
   imagesetfile = os.path.join(
            self._devkit_path,
            # 'VOC' + self._year,
            'bottle',
            'ImageSets',
            'Main',
            self._image_set + '.txt')
   ```
