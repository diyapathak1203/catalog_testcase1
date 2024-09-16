# Shamir's Secret Sharing: Polynomial Constant Term Finder

## Overview

This repository contains a JavaScript implementation for finding the constant term of a polynomial using Shamir's Secret Sharing algorithm. The project involves decoding polynomial roots encoded in various bases and performing polynomial interpolation to compute the constant term.

## Features

- **JSON Parsing**: Read polynomial roots from a JSON file.
- **Base Conversion**: Decode values from different bases to decimal.
- **Polynomial Interpolation**: Compute the polynomial's constant term using Lagrange interpolation.
- **Precision Handling**: Manage floating-point precision issues.

## Setup and Installation

### Prerequisites

- **Node.js**: Ensure Node.js is installed. Download it from [Node.js official website](https://nodejs.org/).

### Clone the Repository

1. Open your terminal or command prompt.

2. Clone the repository using Git:

   ```sh
   git clone https://github.com/diyapathak1203/catalog_testcase1.git
## Running the Project

### Step-by-Step Instructions

1. **Navigate to the Project Directory**

   Open your terminal or command prompt and change to the project directory using:

   ```sh
   cd polynomial_project
   
 2.**Initialize the Node.js Project**

Ensure you have created a package.json file by running:

```sh
npm init -y
This command will generate a default package.json file required for managing Node.js dependencies.

3.**Create the Input File**

In the project directory, create a file named input.json and add your JSON input data. For example:

```json

{
    "keys": {
        "n": 9,
        "k": 6
    },
    "1": {
        "base": "10",
        "value": "28735619723837"
    },
    "2": {
        "base": "16",
        "value": "1A228867F0CA"
    },
    "3": {
        "base": "12",
        "value": "32811A4AA0B7B"
    },
    "4": {
        "base": "11",
        "value": "917978721331A"
    },
    "5": {
        "base": "16",
        "value": "1A22886782E1"
    },
    "6": {
        "base": "10",
        "value": "28735619654702"
    },
    "7": {
        "base": "14",
        "value": "71AB5070CC4B"
    },
    "8": {
        "base": "9",
        "value": "122662581541670"
    },
    "9": {
        "base": "8",
        "value": "642121030037605"
    }
}
4.**Add the JavaScript Code**

Create a file named shamir.js in the project directory and add the following code:

```javascript
const fs = require('fs');

function decodeValue(value, base) {
    return parseInt(value, base);
}

function lagrangeInterpolation(points, x) {
    let total = 0;
    let n = points.length;

    for (let i = 0; i < n; i++) {
        let xi = points[i].x;
        let yi = points[i].y;
        let li = 1;

        for (let j = 0; j < n; j++) {
            if (i !== j) {
                li *= (x - points[j].x) / (xi - points[j].x);
            }
        }
        total += li * yi;
    }

    return total;
}

const path = './input.json';
try {
    const inputData = fs.readFileSync(path, 'utf8');
    const input = JSON.parse(inputData);

    const { n, k } = input.keys;
    let points = [];

    for (let key in input) {
        if (key !== 'keys') {
            let { base, value } = input[key];
            let x = parseInt(key);
            let y = decodeValue(value, parseInt(base));
            points.push({ x, y });
        }
    }

    if (points.length < k) {
        throw new Error(`Not enough points provided. Required: ${k}, Given: ${points.length}`);
    }

    const constantTerm = lagrangeInterpolation(points, 0);
    const roundedConstantTerm = Math.round(constantTerm * 1e10) / 1e10;
    console.log('Constant Term:', roundedConstantTerm);

} catch (error) {
    console.error('Error reading or parsing JSON file:', error.message);
}
5.**Run the Script**

Execute the script with Node.js to compute the constant term:

```sh

node shamir.js
This command will read from input.json, decode the values, perform polynomial interpolation, and print the constant term to the console.

Example Output
When you run the script, you might see an output like:


Constant Term: 2.9999999999999987
This indicates that the constant term of the polynomial is approximately 3.0.




