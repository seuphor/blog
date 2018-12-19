---
title: Virtual Environment Setup
date: 2018-12-18 22:44:00
categories:
- tutorials
tags:
- setup
---

This is a setup guide for virtualenv in Ubuntu environment

## 1. Installation

(I). pip for Python

```sh
# update package list
sudo apt update

# install python3 pip
sudo apt install python3-pip

# check the pip version
pip3 --version
```

(II). virtualenv package 

```sh
pip3 install virtualenv
```

## 2. Setup virtualenv

```sh
mkdir myproject
cd myproject

virtualenv -p /path/to/python venv
touch .gitignore # add /venv to ignore the folder

pip install some_package
pip freeze [--local] > requirements.txt
# pip install -r requirements.txt
```