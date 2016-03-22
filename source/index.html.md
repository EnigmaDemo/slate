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

Enigma is a distributed P2P cloud which allows storage and computation of data in encrypted form.

This guide will help you intiate an enigma instance, securely store data, set access control policies and perform basic statistics and linear algebra operations on data. 


# Deployment

Enigma nodes can be installed on Linux server using a docker image. First, set up an initial seed node. Additional nodes can join given the address of the seed node, propagation to the rest of the network happens automaticlly.

## Start Seed Node

```python
docker run enigma 6655
```

Creates a new enigma instance.

`docker run -seed enigma [port]`

### Start parameters
Parameter | Type | Description
--------- | ------- | -----------
port | integer | local port of node

## Add Node 

```python
docker run enigma 24.133.12.31 6655
```

Starts new node to connect to current Enigma instance.

`docker run enigma [ip] [port]`

### Start parameters
Parameter | Type | Description
--------- | ------- | -----------
ip | integer | ip of seed node
port | integer | port of seed node


# Key Management 

Each node on the network has an identity represenented by a private/public key pair. The range of valid private keys is governed by the secp256k1 ECDSA standard used by Bitcoin.

## Generate Key

```python
private_key, public_key = enigma.gen_key()
```

Generates a private/public key pair and sets them for the local node.

`enigma.gen_keys()`

**Returns:** 
 *string* a 'private_key', *string* the corresponding `public_key`


## Get Public Key

```python
public_key = enigma.get_pubkey()
```
Gets the public key corresponding to the currently set private key.

`enigma.get_pubkey()`

**Returns:** *string* the `public_key`.


## Set Key


```python
enigma.set_privkey(privkey)
```
Sets the current identity to a specified private key

`enigma.set_privkey(privkey)`

**Returns:** *boolean* `true` if successful, `false` otherwise.

### Set private key parameters
Parameter | Type | Description
--------- | ------- | -----------
privkey | string | private key associated to the data stored in enigma


# Data Management 

Enigma uses Shamir's Secret Sharing Scheme for splitting data into encrypted pieces. Different nodes in the network recieve different pieces, and therefore cannot decrypt the data. The encryption and storage are all done in the background. The key associated with the data is the sha256 hash of it.

## Store Data

```python
// model age
age = 24
key_age = enigma.store(age) 

// model hip, waist, and bust dimensions, month over month
body = [[90,60,90][88,57,89][92,64,91]]
key_body = enigma.store(body) 
```

Securely stores data in the enigma cloud. 

`enigma.store(data)`

**Returns:** *string* `key` referencing the data, `error` if undable to store the data.

### Store parameters
Parameter | Type | Description
--------- | ------- | -----------
data | array[] | data to securely store in enigma, data can be in any format 


## Retreive Data

```python
my_key = "c2356069e9d1e79ca924378153cfbbfb4d4416b1f99d41a2940bfdb66c5319db"
data = enigma.load(my_key)
```

Extracts the data from the enigma cloud and reconstructs it in clear.

`enigma.load(key)`

**Returns:** *array* list with the data `data`, `error` otherwise.

### Load parameters

Parameter | Type | Description
--------- | ----------- | -----------
key | string | key pointing to data


## Delete Data

```python
key = "c2356069e9d1e79ca924378153cfbbfb4d4416b1f99d41a2940bfdb66c5319db"
enigma.delete(key)
```

Deletes data from the engima cloud.

`enigma.delete(key)`

**Returns:** *boolean* `true` if data sucessfully deleted, `false` otherwise.

### Delete parameters
Parameter | Type | Description
--------- | ----------- | -----------
key | string | key pointing to data 




# Access Control

Access control is managed on the bitcoin blockchain. Access policies for each identity are repsesented by a blockchain transaction, intitiatied by the data owner. For more information, see <a href = 'http://web.media.mit.edu/~guyzys/data/ZNP15.pdf'> Decentralizing Privacy: Using Blockchain to Protect
Personal Data </a>


## Grant Access

```python
addresses = ['1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD', 
'1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2', 
'3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy']
transaction_id = enigma.pair(addresses) 
```


Data owner grant access to a set of specified addresses.

`enigma.pair(public_addresses)`

**Returns:** *string* `transaction_id` of user access control policy, `error` otherwise.

### Pairing parameters
Parameter | Type | Description
--------- | ------- | -----------
public_addresses | array[string] | addresses to grant access


## Revoke Access

```python
addresses = ['1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD', 
'1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2', 
'3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy']
transaction_id = enigma.revoke(addresses) 

```

Lets the data owner revoke access for a set of specified addresses. 

`enigma.revoke(public_addresses)`

**Returns:** *string* `transaction_id` of user access control policy, `error` otherwise.

### Unpair parameters
Parameter | Type | Description
--------- | ------- | -----------
public_addresses | array[string] | addresses to remove access

# Data Processing 

## Max

```python
alice_net_worth = 10000000
bob_net_worth = 30000
alice_key = enigma.store(alice_net_worth)
bob_key = enigma.store(bob_net_worth)
max_value = enigma.stats.max(alice_key, bob_key)
if max value == alice_key:
	print 'Alice is richer'
else if max value == bob_key:
	print 'Bob is richer'
```

Finds the max value of a data set.

`enigma.stats.max(keys)`

**Returns:** *string* key pointing to the max value.

### Min parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys that point to data 


## Min

```python
alice_net_worth = 10000000
bob_net_worth = 30000
alice_key = enigma.store(alice_net_worth)
bob_key = enigma.store(bob_net_worth)
min_value = enigma.stats.min(alice_key, bob_key)
if min value == alice_key:
	print 'Alice is poorer'
else if in value == bob_key:
	print 'Bob is poorer'
```

Finds the min value in a data set.

`enigma.stats.min(keys)`

**Returns:** *string* key pointing to the min value.

### Min parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys pointing to data 


## Mean

```python
ages_of_customers = [45,43,23,15,67,32,28,98,55,14]
key = enigma.store(ages_of_customers)
mean = enigma.stats.mean(key)
```

Computes the mean of a given data set.

`engima.stats.medan(keys)`

**Returns:** *array* a new array containing the mean value. 

### Mean parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys pointing to data 


## Standard deviation


```python
ages_of_customers = [45,43,23,15,67,32,28,98,55,14]
key = enigma.store(ages_of_customers)
standard_dev = enigma.stats.std(key)
```

Computes the standard deviation over a given data set. 

`enigma.stats.std(keys)`

**Returns:** *array* a new array containing the standard deviation.

### Standard deviation parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys pointing to data 


## Variance


```python
ages_of_customers = [45,43,23,15,67,32,28,98,55,14]
key = enigma.store(ages_of_customers)
variance = enigma.stats.var(key)
```

Computes the variance over a given data set. The variance is computed for the flattened array.

`enigma.stats.var(keys)`

**Returns:** *array* a new array containing the variance.

### Variance parameters
Parameter | Type | Description
--------- | ----------- | -----------
keys | array[string] | keys pointing to data 



## Linear regression

```python
key_a = "10bb65d718d7cc446d2d155ed9ccb2812a82c3c447e8333d5aebfed541874c44"
key_b = "34eb5849b302e887bc41e516e9bd46c8629af2943c4da4aa94e4d2877fc16b81"
slope, intercept, r_value, p_value, std_err = enigma.stats.linregress(key_a, key_b)
```

Computes the linear regression for a given data set using ordinary least squares method.

`enigma.stats.linregress(key_a, key_b)`

**Returns:** 
* `slope`: *float* slope of the regression line 
* `intercept` *float* intercept of the regresion line 
* `r:value`: *float* correlation coefficient
* `p_value`: *float* two-sided p-value for a hypothesis test whose null hypothesis is that the slope is zero
* `stderr`: *float* standard error of the estimate

### Linear regressions parameters
Parameter | Type | Description
--------- | ----------- | -----------
key_a | string | key that points to the first set of data
key_b | string | key that points to the second set of data


## Multiply matrices


```python
key_a = "10bb65d718d7cc446d2d155ed9ccb2812a82c3c447e8333d5aebfed541874c44"
key_b = "34eb5849b302e887bc41e516e9bd46c8629af2943c4da4aa94e4d2877fc16b81"
prod = enigma.linalg.dot(key_a, key_b)
```

Computes the dot product of two arrays. For 2-D arrays it is equivalent to matrix multiplication, and for 1-D arrays to inner product of vectors (without complex conjugation). For N dimensions it is a sum product over the last axis of a and the second-to-last of b.

`enigma.linalg.dot()`

**Returns:** *array* the dot product of a and b, if a and b are both scalars or both 1-D arrays then a scalar is returned; otherwise an array is returned.

### Multiply parameters
Parameter | Type | Description
--------- | ----------- | -----------
key_a | string | key that points to the first array
key_b | string | key that points to the second array


## Inverse of a matrix


```python
key_to_matrix = "10bb65d718d7cc446d2d155ed9ccb2812a82c3c447e8333d5aebfed541874c44"
inverted_matrix = enigma.linalg.inv(key_to_matrix)
```

Compute the (multiplicative) inverse of a matrix.

`enigma.linalg.inv(key)`

**Retunrs:** *array* array with (multiplicative) inverse of the matrix.

### Inverse parameters
Parameter | Type | Description
--------- | ----------- | -----------
key | string | key that has access to the matrix






