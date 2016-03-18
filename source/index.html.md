---
title: API Reference

language_tabs:
  - python

toc_footers:
  - 

includes:
  - 

search: true

---

# Introduction

Welcome to the Enigma API! 
You can use this API to setup enigma nodes, securely store data, and perform operations on the encrypted data.
Enigma is currently available in Python. 
You will find code examples in the dark area to the right.



# Deployment

## Install Seed Node

The Linux server will be used as seed node (i.e. first node in the cloud). 
Installation prerequisites:
1. Server running Linux (there no specific hardware requirements)
2. Have Docker installed, see <a href='https://docs.docker.com/engine/installation/linux/'>instructions here</a>
To deply enigma on the server copy the engima docker image to a folder of your choice and run the docker image.


## Network Confirguration

> To configure the enigma network, use this code:

```python
import json
from pprint import pprint

with open('config.json') as config_file: 
  success = engima.config(config_file)
```

Configures the enigma cloud using a JSON file. Configuration parameters that can be set via the JSON file:
* number of nodes
* redundancy
* GUY ?

engima.config(config_file)

**Returns:** *boolean" `true` if configuration successfuly implemented, `false` otherwise.

### Start parameters
Parameter | Type | Description
--------- | ------- | -----------
config_file | file object | JSON file with configuration parameters


## Add Node 

> To add a new node, use this code:

```python
node_added = enigma.start(10.255.16.8)
```

Adds a new node to the enigma cloud. 

`enigma.start(ip_address)`

**Returns:** *boolean* `true` if node added succesfully, `false` otherwise.

### Start parameters
Parameter | Type | Description
--------- | ------- | -----------
ip_address | string | ip address of the seed node to connect to
<aside class="notice">
Requires having first deployed enigma on a seed node. See instructions above on how to do this.  
</aside>


# Access Control

## Grant Access

> To grant access to data, use this code:

```python
addresses = ['1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD', '1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2', '3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy']
transaction_id = enigma.pair(addresses) 
```

Grant access to data associated with a set of specified addresses. Addresss are compatible with bitcoin addresses.

`enigma.pair(public_addresses)`

**Returns:** The transaction_id` in the blockchain that has acceess control information, `false` if pairng is unsucessful.

### Pairing parameters
Parameter | Type | Description
--------- | ------- | -----------
public_addresses | array[string] | addresses referencing data to grant access to 


## Revoke Access

> To revoke access to data use this code:

```python
addresses = ['1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD', '1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2', '3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy']
revoke_successful = enigma.revoke(addresses) 
```

Revokes data access associated to a set of specified addresses. 

`enigma.revoke(public_addresses)`

**Returns:** returns the `transaction_id` in the blockchain, `error` otherwise.

### Unpair parameters
Parameter | Type | Description
--------- | ------- | -----------
public_addresses | array[string] | addresses to cut acccess to



# Key Management 

## Generate Key

> To generate a key pair, use this code:

```python
private_key, public_key = enigma.gen_key()
```

Generates a private/public key pair. 

enigma.gen_key()

**Returns:** 
* *string* `private_key`
* *string* public_key`


## Get Public Key

> To get he public key, use the following code:

```python
public_key = enigma.get_pubkey()
```
Gets the public key corresponding to the current private key set.

enigma.get_tpubkey()

**Returns:** *string* a `public_key`.


## Set Key

> to set the private key, use the following code:

```python
keys_is_set = enigma.set_privkey(privkey)
```
Sets the key to a specified private key.

enigma.set_privkey(private_key)

**Returns:** *boolean* `true` if set is successful, `false otherwise.

### Set private key parameters
Parameter | Type | Description
--------- | ------- | -----------
privkey | string | private key assocaited to the data stored in enigma


# Data Management 

## Store Data
> To store data in the enigma cloud, use this code:

```python
data = [24.7, 115.19, 668.1, 12.3, 9.89, 245.4, 110.2, 3.58, 6.71, 2.35, 34.5, 330]
key = enigma.store(data) 
```

Securely stores data in the enigma cloud. 

`enigma.store(data)`

**Returns:** `key` referencing the data, `error` if undable to store the data.

### Store parameters
Parameter | Type | Description
--------- | ------- | -----------
data | array[] | data to securely store in enigma, data can be in any format 


## Retreive Data

> To retreive data from the enigma cloud, use this code:

```python
my_key = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
data = enigma.load(my_key)
```

Extracts the data from the enigma cloud and reconstructs it in clear.

`enigma.load(key)`

**Returns:** *array* list of `data` if access is permitted, `error` otherwise.

### Load parameters

Parameter | Type | Description
--------- | ----------- | -----------
key | string | key used to access the data 


## Delete Data

> To delete data from the enigma cloud, use this code:

```python
key = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
data_deleted = enigma.delete(key)
```

Deletes data from the engima cloud.

`enigma.delete(key)`

**Returns:** *boolean* `true` if data sucessfully deleted, `false` otherwise.

### Delete parameters
Parameter | Type | Description
--------- | ----------- | -----------
key | string | key used to access the data 



# Data Processing 

## Minimum

> To find the min vaue in an array, use the following code:

``python
key_to_data = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
min_value = enigma.stats.min(key_to_data)
```

Finds the minimum value of a data set

enigma.stats.min(keys)

**Returns:** *float* miniumum value of an array.

### Min parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys that have access to the data 


## Maximum

>To find the max vaue in an array, use the following code:

```python
key_to_data = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
max_value = enigma.stats.max(key_to_data)
```

Finds the maximum value of a data set

enigma.stats.max(keys)

**Returns:** *float* maxium value of an array.

### Max parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys that have access to the data 


## Percentile

> To compute the 25th percentile of a data set, use the following code:

```python
percentile_to_compute = 25
key_to_data = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
percentile = enigma.stats.percentile(key_to_data, percentile_to_compute)
```

Compute the qth percentile of the data along the specified axis.

enigma.stats.percentile(keys, q)

Returns | Type | Description
--------- | ----------- | -----------
percentile | scalar | percentile value, if the input contains integers, or floats of smaller precision than 64, then the output data-type is float64, otherwise, the output data-type is the same as that of the input

### Percentile parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys that have access to the data 
q | float | percentile to compute which must be between 0 and 100 inclusive.


## Median

> To compute the median, use the following code:

```python
key_to_data = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
median = enigma.stats.median(key_to_data)
```

Computes the median of a given data set. 

engima.stats.median(keys)

**Returns:** *float* `median` value.

### Median parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys that have access to the data 


## Mean

> To compute the mean, use the following code:

```python
key_to_data = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
mean = enigma.stats.mean(key_to_data)
```

Computes the mean of a given data set.

engima.stats.medan(keys)

**Returns:** *array* a new array containing the mean value. 

### Mean parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys that have access to the data 


## Standard deviation

> To compute the standard deviation, use the following code:

```python
key_to_data = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
standard_dev = enigma.stats.std(key_to_data)
```

Computes the standard deviation over a given data set. 

enigma.stats.std(keys)

**Returns:** *array* a new array containing the standard deviation.

### Standard deviation parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys that have access to the data 


## Variance

> To compute the variance, use this code:

```python
key_to_data = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
variance = enigma.stats.var(key_to_data)
```

Computes the variance over a given data set, which is a measure of the spre The variance is computed for the flattened array.ad of a distribution.  The variance is computed for the flattened array.

enimga.stats.var(keys)

**Returns:** *array* a new array containing the variance.

### Variance parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys that have access to the data 


## Correlate

> To compute the correlation, use the following code:

```python
key_a = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
key_b = "EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F4000013048024100C918FACF8DEB2DEFD5FD3789B9E069"
corr = enigma.stats.correlate(key_a, key_b)
```

Computes cross correlation of two 1-dimensional sequences. Sequences are zero-padded where necessary. Uses the same algorithm as numpy.correlate¶ python function.

enimga.stats.correlate(key_a, key_b)

**Returns:** *array* discrete cross-correlation of data associated to `key_a` and `key_b`.

### Correlate parameters
Parameter | Type | Description
--------- | ----------- | -----------
key_a | string | key that has access to the first data set
key_b | string | key that has access to the second data set 


## Covariance

> To compute the covariance, use the following code:

```python
key_to_data = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
covariance = enigma.stats.cov(key_to_data)
```
Estimates a covariance matrix, given data and weights. Covariance indicates the level to which two variables vary together. Uses the same algorithm as numpy.cov¶ python function.

engima.stats.cov(key)

**Returns:** *array* covariance matrix of the variables.

### Covariance parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | string | key that has access to the data, which should be a 1-D or 2-D array containing multiple variables and observations where each row of m represents a variable, and each column a single observation of all those variables


## Linear regression

> To compute a linear regression, use the following code:

```python
key_a = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
key_b = "EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F4000013048024100C918FACF8DEB2DEFD5FD3789B9E069"
slope, intercept, r_value, p_value, std_err = enigma.stats.linregress(key_a, key_b)
```

Computes the linear regression for a given data set using ordinary least squares method.

enimga.stats.linregress(key_a, key_b)

**Returns:** 
* `slope`: *float* slope of the regression line 
* `intercept` *float* intercept of the regresion line 
* `r:value`: *float* correlation coefficient
* `p_value`: *float* two-sided p-value for a hypothesis test whose null hypothesis is that the slope is zero
* `stderr`: *float* standard error of the estimate

### Linear regressions parameters
Parameter | Type | Description
--------- | ----------- | -----------
key_a | string | key that has access to the first set of data
key_b | string | key that has access to the second set of data


## Multiply matrices

> To mulitply two matrices, use the following code:

```python
key_a = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
key_b = "EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F4000013048024100C918FACF8DEB2DEFD5FD3789B9E069"
 = enigma.linalg.dot(key_a, key_b)
```

Computes the dot product of two arrays. For 2-D arrays it is equivalent to matrix multiplication, and for 1-D arrays to inner product of vectors (without complex conjugation). For N dimensions it is a sum product over the last axis of a and the second-to-last of b.

enigma.linalg.dot()

**Returns:**  *array* the dot product of a and b, if a and b are both scalars or both 1-D arrays then a scalar is returned; otherwise an array is returned.

### Correlate parameters
Parameter | Type | Description
--------- | ----------- | -----------
key_a | string | key that has access to the first array
key_b | string | key that has access to the second array


## Inverse of a matrix

> To compute the inverse of one matrix, use this code:

```python
key_to_matrix = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
inverted_matrix = enigma.linalg.inv(key_to_matrix)
```

Compute the (multiplicative) inverse of a matrix.

enigma.linalg.inv(key)

**Retunrs:** *array* array with (multiplicative) inverse of the matrix.

### Inverse parameters
Parameter | Type | Description
--------- | ----------- | -----------
key | string | key that has access to the matrix

<aside class="notice">
Will only run if keys provided have authorized access (also assumes addresses associated to data still have access?)
</aside>



