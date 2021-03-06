LIBLINEAR for Go
==========

[![Build Status](https://travis-ci.org/lazywei/liblinear.svg?branch=master)](https://travis-ci.org/lazywei/liblinear)

This is a Golang wrapper for [LIBLINEAR (C.-J. Lin et al.)](http://ntucsu.csie.ntu.edu.tw/~cjlin/liblinear/) ([GitHub](https://github.com/cjlin1/liblinear)).
Note that the interface of this package might be slightly different from
liblinear C interface because of Go convention. Yet, I'll try to align the
function name and functionality to liblinear C library.

**GoDoc**: [Document](https://godoc.org/github.com/lazywei/liblinear).

## Introduction to LIBLINEAR

LIBLINEAR is a linear classifier for data with millions of instances and features. It supports

- L2-regularized classifiers
- L2-loss linear SVM, L1-loss linear SVM, and logistic regression (LR)
- L1-regularized classifiers (after version 1.4)
- L2-loss linear SVM and logistic regression (LR)
- L2-regularized support vector regression (after version 1.9)
- L2-loss linear SVR and L1-loss linear SVR.


## Install

This package depends on LIBLINEAR 2.01+. Please install it first via Homebrew or
other package managers on your OS:

```
brew update
brew info liblinear # make sure your formula will install version higher than 2.01
brew install liblinear
```

After liblinear installation, just `go get` this package

```
go get github.com/lazywei/liblinear
```

## Usage

*The package is based on [mat64](https://godoc.org/github.com/gonum/matrix/mat64).*

```go
import linear "github.com/lazywei/liblinear"

// ReadLibsvm(filepath string, oneBased bool) (X, y *mat64.Dense)
X, y := linear.ReadLibsvm("heart_scale", true)

// Train(X, y *mat64.Dense, bias float64, solverType int,
// 	eps, C_, p float64,
// 	classWeights map[int]float64) (*Model)
// Please checkout liblinear's doc for the explanation for these parameters.
model := Train(X, y, -1, 0, 0.01, 1.0, 0.1, map[int]float64{1: 1, -1: 1})
y_pred:= linear.Predict(model, X)

fmt.Println(linear.Accuracy(y, y_pred))
```

## Self-Promotion

This package is mainly built because of
[mockingbird](https://github.com/lazywei/mockingbird), which is a programming
language classifier in Go. Mockingbird is my Google Summer of Code 2015 Project
with GitHub and [linguist](https://github.com/github/linguist). If you like it,
please feel free to follow linguist, mockingbird, and this library.

## Roadmap

- [ ] Wrap core functions
  - [x] Train
  - [x] Predict
  - [x] LIBSVM format reader
  - [x] Predict Probability
  - [ ] Cross Validation
  - [x] Save Model / Load Model
- [ ] Better Wrapper for liblinear's C struct
  - [x] Model (wrap `struct model`)
  - [x] FeatureNode (wrap `struct feature_node`)
  - [x] Parameter (wrap `struct parameter`)
  - [ ] Problem (wrap `struct problem`)
- [ ] Abstraction for classifier: encapsulate `model`
