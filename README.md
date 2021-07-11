<p align="center">
  <a href="https://github.com/tkestack/knitnet-operator">
    <img src="https://github.com/tkestack/knitnet-operator/workflows/CI%20Pipeline/badge.svg" alt="Github CI">
  </a>
  <a href="https://goreportcard.com/report/github.com/tkestack/knitnet-operator">
    <img src="https://goreportcard.com/badge/github.com/tkestack/knitnet-operator" alt="GoReportCard">
  </a>
  <a href="https://quay.io/repository/danielxlee/knitnet-operator">
    <img src="https://img.shields.io/badge/container-ready-green" alt="Docker">
  </a>
  <a href="https://github.com/tkestack/knitnet-operator/master/LICENSE">
    <img src="https://img.shields.io/badge/License-Apache%202.0-blue.svg" alt="License">
  </a>
</p>

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Knitnet Operator](#knitnet-operator)
  - [Architecture](#architecture)
    - [Purpose](#purpose)
    - [Supported Features](#supported-features)
  - [Getting Started](#getting-started)
    - [Example](#example)
    - [Prerequisites](#prerequisites)
    - [Quickstart](#quickstart)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Knitnet Operator

A Golang based knitnet operator that will make/oversee Submariner components on top of the Kubernetes.

## Architecture

<div align="center">
    <img src="./docs/icons/submariner-arch.png">
</div>

### Purpose

The purpose of creating this operator was to provide an easy and production-grade setup of Submariner components on Kubernetes. It doesn't care if you have a plain on-prem Kubernetes or cloud-based.

### Supported Features

Here the features which are supported by this operator:-

- Deploy submariner broker
- Join cluster to broker
- Check k8s server version
- Support cloud prepare (aws, gcp)
- Support lighthouse, globalnet enable/disable

## Getting Started

### Example

The configuration of Knitnet setup should be described in Knitnet CRD. You will find all the examples manifests in [example](./config/samples) folder.

### Prerequisites

Knitnet operator requires a Kubernetes cluster of version `>=1.5.0`. If you have just started with Operators, its highly recommended to use latest version of Kubernetes. And the prepare 2 cluster, example `cluster-a` and `cluster-b`

### Quickstart

The setup can be done by using `kustomize`.

1. Clone source code

    ```shell
    git clone https://github.com/tkestack/knitnet-operator.git
    ```

1. Deploy broker

    - Install knitnet operator

      ```shell
      kubectl config use-context cluster-a
      cd knitnet-operator
      make deploy
      ```

    - Deploy broker on `cluster-a`
  
      ```shell
      kubect -n knitnet-operator-system apply -f .config/samples/deploy_broker.yaml
      ```

    - Export `submariner-broker-info` configmap to a yaml file

      ```shell
      kubectl -n submariner-k8s-broker get cm submariner-broker-info -oyaml > submariner-k8s-broker.yaml
      ```

2. Join cluster to broker

     - Install knitnet operator

       ```shell
       kubectl config use-context cluster-b
       make deploy
       ```

     - Create `submariner-broker-info` configmap

       ```shell
       kubectl apply -f submariner-k8s-broker.yaml
       ```

     - Join `cluster-a` to `cluster-b`

       ```shell
       kubectl -n knitnet-operator-system apply -f .config/samples/join_broker.yaml
       ```
