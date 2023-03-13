# 1. Introduction

This document provides the technical specification for the interaction between the components of the Xixels consensus network, based on the Xixels whitepaper.

This section introduces the various components and basic concepts of the Xixels consensus network.

## 1.1 Leader Node

The Leader Node is the primary node in the consensus network. It provides all services related to state updates. In the current protocol version, there is only one Leader Node globally, which is operated by an operator authorized by Xixels DAO.

The main functions of the Leader Node are as follows:

- Degas metadata management;
- Provides the Kubernetes environment required for running the Degas server-side image;
- Provides database services for Degas use;
- Provides Gateway to forward RPC services and Message Services between the Client and Degas;
- Provides data synchronization service with Follower;

## 1.2 Follower Node

There can be any number of Follower Nodes in the consensus network. Follower Nodes are secondary nodes. A Follower Node can synchronize data from the Leader Node or other Follower Nodes.

This data is the basis for developers and communities to verify the Degas status.

Currently, the data synchronized among the nodes includes:

- User account information
- Degas deployment information
- Degas RPC service requests and results
- Message Service messages
- Database data


## 1.3 Degas

Degas is an application developed by developers deployed on Xixels.

A Degas program usually consists of two parts: server-side program and client-side program.

The server-side program is a Kubernetes image and runs in a Kubernetes container on the Leader Node. The Server-side program can provide JSON RPC and Message Service services, and can also access the services of other Degas. The Leader Node also provides database services for the server-side program.

The client-side program is a program that runs on the Client. The client-side program is built on the Xixels game engine, and access the RPC and Message services provided by the Server-side program.

For more information about Degas, please refer to [here](/protocol/degas.md).

## 1.4 Client

The client is the entry point for end users to access Degas games. The client downloads the Degas client-side program from the Leader Node and runs it. The most important component in the Client is Xixels game engine. For more information about Xixels game engine, please refer to [here]().
