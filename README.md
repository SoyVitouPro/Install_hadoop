# Hadoop 3.3.6 Single-Node (Pseudo-Distributed) Setup Guide

This guide explains how to set up Hadoop 3.3.6 in **single-node (pseudo-distributed) mode**.  
It runs all Hadoop daemons (NameNode, DataNode, ResourceManager, NodeManager) on one machine.

---

## 1. Prerequisites
- Linux (Ubuntu/Debian recommended)
- Java 8 or 11 installed
- SSH enabled (localhost without password)
- User `hadoop` created (non-root)

Check:
```bash
java -version
ssh localhost
