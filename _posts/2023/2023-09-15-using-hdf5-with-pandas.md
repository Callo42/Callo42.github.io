---
layout: post
title: Using HDF5 With Pandas
date: 2023-09-15 14:05 +0800
categories: [Gleanings, Technical Supports] 
tags: [Pandas, HDF5]
math: true
---


## Using HDFStore (Pandas)
### HDFStore
- HDFStore is a [Pandas](https://pandas.pydata.org/) library that offering some top-level operations on [HDF5](https://www.hdfgroup.org/) files with [Pytables](https://www.pytables.org/) as its base implementation.
- Some useful Pandas official HDFStore documentations could be found at:
  - <https://pandas.pydata.org/docs/reference/io.html#hdfstore-pytables-hdf5>, several most frequently used api.
  - <https://pandas.pydata.org/docs/user_guide/io.html#hdf5-pytables>, a detailed documentation introducing the most fundamental operations using HDFStore.
  - <https://pandas.pydata.org/docs/user_guide/cookbook.html#cookbook-hdf>, A cookbook containing HDFStore usage, including ``how to add attributes to dataset`` for example.

### Some basic operations:

#### Creating HDF5 File and Store Data into it

- The ``HDFStore`` method open ups a HDF5 file with *append* mode as default, and could be used with the ``with`` syntax. (Automatically close the file after writing)

```python
In [1]: import pandas as pd

In [3]: df = pd.DataFrame([[1,2],[3,4]],columns=['A','B'])

# The data could be saved hierarchically, for example
In [4]: with pd.HDFStore('test.hdf5') as store:
   ...:     store.put('/group1/df1',df) # This stores the df DataFrame into the group group1, and naming as dataset df1.

# The with method closes the file, as shown below:
In [5]: print(store.info())
<class 'pandas.io.pytables.HDFStore'>
File path: test.hdf5
File is CLOSED

In [7]: import numpy as np

In [8]: df2 = pd.DataFrame(np.array(range(10)))

# Save another data hierarchically
In [9]: with pd.HDFStore('test.hdf5') as store:
   ...:     store.put('/group2/df2',df2)
   ...:
```

- Then we can open the ``test.hdf5`` file with directly calling ``HDFStore`` method. Remember to close it by ``store.close()`` to make changes applied to disk.

  - The ``HDFStore`` method typically accepts two parameters:
    -  ``path`` as the hdf5 file's path
    -  ``mode``: (default `a`)
       - `r` for read-only
       - `w` for write (an existing file with the same name would be deleted)
       - `a` for append 
  - The full docstrings for this functions could be yield by calling ``help()`` in python:
    ```python
    class HDFStore(builtins.object)
    |  HDFStore(path, mode: 'str' = 'a', complevel: 'int | None' = None, complib=None, fletcher32: 'bool' = False, **kwargs) -> 'None'
    |
    |  Dict-like IO interface for storing pandas objects in PyTables.
    |
    |  Either Fixed or Table format.
    |
    |  .. warning::
    |
    |     Pandas uses PyTables for reading and writing HDF5 files, which allows
    |     serializing object-dtype data with pickle when using the "fixed" format.
    |     Loading pickled data received from untrusted sources can be unsafe.
    |
    |     See: https://docs.python.org/3/library/pickle.html for more.
    |
    |  Parameters
    |  ----------
    |  path : str
    |      File path to HDF5 file.
    |  mode : {'a', 'w', 'r', 'r+'}, default 'a'
    |
    |      ``'r'``
    |          Read-only; no data can be modified.
    |      ``'w'``
    |          Write; a new file is created (an existing file with the same
    |          name would be deleted).
    |      ``'a'``
    |          Append; an existing file is opened for reading and writing,
    |          and if the file does not exist it is created.
    |      ``'r+'``
    |          It is similar to ``'a'``, but the file must already exist.
    |  complevel : int, 0-9, default None
    |      Specifies a compression level for data.
    |      A value of 0 or None disables compression.
    |  complib : {'zlib', 'lzo', 'bzip2', 'blosc'}, default 'zlib'
    |      Specifies the compression library to be used.
    |      As of v0.20.2 these additional compressors for Blosc are supported
    |      (default if no compressor specified: 'blosc:blosclz'):
    |      {'blosc:blosclz', 'blosc:lz4', 'blosc:lz4hc', 'blosc:snappy',
    |       'blosc:zlib', 'blosc:zstd'}.
    |      Specifying a compression library which is not available issues
    |      a ValueError.
    |  fletcher32 : bool, default False
    |      If applying compression use the fletcher32 checksum.
    |  **kwargs
    |      These parameters will be passed to the PyTables open_file method.
    ...
    ```
- We can print out the HDF5 file's structure with ``store.info()``

    ```python
    In [10]: store = pd.HDFStore('test.hdf5')

    In [11]: print(store.info())
    <class 'pandas.io.pytables.HDFStore'>
    File path: test.hdf5
    /group1/df1            frame        (shape->[2,2])
    /group2/df2            frame        (shape->[10,1])
    ```

- On the left we can see the hierarchy of the groups added to the storage, in the middle we have the type of dataset and on the right there is the list of attributes attached to the dataset. Attributes are pieces of metadata you can stick on objects in the file and the attributes we see here are automatically created by Pandas in order to describe the information required to recover the data from the hdf5 storage system.

### Adding Attributes to Dataset

To add attributes to a dataset:

- One needs to use ``get_storer()`` method and asigning new metadata into the attributs by accessing ``store.get_storer('datapath').attrs``:


    ```python
    # This firstly add an attribute named metadata to the storer's attributes, then assign an attribute for metadata with key-value pair 'attr1':1
    In [12]: store.get_storer('/group1/df1').attrs.metadata = {'attr1':1}

    # To see all the attributes of a storer:
    In [13]: store.get_storer('/group1/df1').attrs
    Out[13]:
    /group1/df1._v_attrs (AttributeSet), 13 attributes:                                                      [CLASS := 'GROUP',
        TITLE := '',
        VERSION := '1.0',
        axis0_variety := 'regular',
        axis1_variety := 'regular',
        block0_items_variety := 'regular',
        encoding := 'UTF-8',
        errors := 'strict',
        metadata := {'attr1': 1}, # Here is the new attribute named metadata we just added.
        nblocks := 1,
        ndim := 2,
        pandas_type := 'frame',
        pandas_version := '0.15.2']
    ```

- And attributes could be deleted by ``del``:
    ```python
    del store.get_storer('/group/df1').attrs.metadata
    ```

    This deletes the metadata attribute for 'df1'.

## References
1. <https://dzone.com/articles/quick-hdf5-pandas#:~:text=We%20can%20create%20a%20HDF5%20file%20using%20the,a%20dataset%20into%20the%20file%20we%20just%20created%3A>
2. <https://en.moonbooks.org/Articles/How-to-add-metadata-to-a-data-frame-with-pandas-in-python-/#:~:text=To%20save%20a%20pandas%20data%20frame%20with%20metadata,%3D%20%7B%27scale%27%3A0.1%2C%27offset%27%3A15%7D%20store.get_storer%20%28%27dataset_01%27%29.attrs.metadata%20%3D%20metadata%20store.close%20%28%29>
3. <https://pandas.pydata.org/docs/reference/io.html#hdfstore-pytables-hdf5>
4. <https://pandas.pydata.org/docs/user_guide/cookbook.html#cookbook-hdf>
5. <https://pandas.pydata.org/docs/user_guide/io.html#hdf5-pytables>
