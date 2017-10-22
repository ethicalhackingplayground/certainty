# Certainty - CA-Cert Automation for PHP Projects

[![Build Status](https://travis-ci.org/paragonie/certainty.svg?branch=master)](https://travis-ci.org/paragonie/certainty)
[![Latest Stable Version](https://poser.pugx.org/paragonie/certainty/v/stable)](https://packagist.org/packages/paragonie/certainty)
[![Latest Unstable Version](https://poser.pugx.org/paragonie/certainty/v/unstable)](https://packagist.org/packages/paragonie/certainty)
[![License](https://poser.pugx.org/paragonie/certainty/license)](https://packagist.org/packages/paragonie/certainty)
[![Downloads](https://img.shields.io/packagist/dt/paragonie/certainty.svg)](https://packagist.org/packages/paragonie/certainty)

Automate your PHP projects' cacert.pem management.

**Requires PHP 5.6 or newer.**

### Motivation

Many HTTP libraries require you to specify a file path to a `cacert.pem` file in order to use TLS correctly.
Omitting this file means either disabling certificate validation entirely (which enables trivial man-in-the-middle
exploits), connection failures, or hoping that your library falls back safely to the operating system's bundle.

In short, the possible outcomes are (from best to worst) are as follows:

1. Specify a cacert file, and you get to enjoy TLS as it was intended. (Secure.)
2. Omit a cacert file, and the OS maybe bails you out. (Uncertain.)
3. Omit a cacert file, and it fails closed. (Connection failed. Angry customers.)
4. Omit a cacert file, and it fails open. (Data compromised. Hurt customers. Expensive legal proceedings.)

Obviously, the first outcome is optimal. So we built *Certainty* to make it easier to ensure open
source projects do this.

## Installing Certainty

From Composer:

```bash
composer require paragonie/certainty:dev-master
```

Due to the nature of CA Certificates, you want to use `dev-master`. If a major CA gets compromised and
their certificates are revoked, you don't want to continue trusting these certificates.

## What Certainty Does

Certainty maintains a repository of all the `cacert.pem` files, along with a 

## Using Certainty

### Create Symlink to Latest CACert

After running `composer update`, simply run a script that excecutes the following.

```php
<?php
(new \ParagonIE\Certainty\Fetch())
    ->getLatestBundle()
    ->createSymlink('/path/to/cacert.pem');
```

Then, make sure your HTTP library is using the cacert path provided. For example, using cURL:

```php
<?php

$ch = curl_init();
//  ... snip ...
curl_setopt($ch, CURLOPT_CAINFO, '/path/to/cacert.perm');
``` 

