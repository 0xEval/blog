---
title: Setting up Android testing with Burp and Genymotion
description: How to setup ADR HTTP interception with Burp (API>=24)
date: 2020-01-15
categories:
  - "Tutorial"
tags:
  - "Burp Suite"
  - "Android"
---

This tutorial is a quick reference on how to setup a Genymotion VM for Android testing with Burp Suite. 
<!--more-->

It used to be quite straight forward but since Nougat (`API 24`), it is required to install your Burp CA (certificate of authority) at system level otherwise it will not be trusted by apps.

## Requirements

1. Genymotion virtualization software (community version is good enough).
2. Emulated device of your choice with API level >= 25 - here we will use a Samsung S7.
3. Your device needs to be **rooted** (easy with VMs) to install Burp's CA at system level.
