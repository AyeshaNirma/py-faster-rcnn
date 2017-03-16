# Train on VOC datasets

## Download VOC

You can refer the [README.md](https://github.com/rbgirshick/py-faster-rcnn#beyond-the-demo-installation-for-training-and-testing-models) for details.

## Problems and Solutions

1. **AssertionError: Selective search data not found at : `\data\selective_search_data\voc_2007_trainval.mat`**

   In the function which has the statement: ` cfg.TRAIN.PROPOSAL_METHOD='' `, add the statement: ` from fast_rcnn.config import cfg `.
   
2. **TypeError: 'numpy.float64' object cannot be interpreted as an index**

   It caused by your numpy version is not match. You should reinstall numpy to 1.11.0
   
   ```make
   pip install -U numpy=1.11.0
   ```
   
3. **No such file or directory: '000001.xml'**

   In `_do_python_eval()` function (`pascal_voc.py`), the result of annopath will always be {:s}.xml. I change the code as follow:
   
   ```python
   annopath = os.path.join(
            self._devkit_path,
            # 'VOC' + self._year,
            'bottle',
            'Annotations',
            '{}.xml')
   ```
   
4. **IndexError: too many indices for array**

   That's because there is nothing detected, because of not enough of iteration or not used pre-trained model. So you can try to change iters.
