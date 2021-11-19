# Polytech coef simulator
 [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
 [![Technologi: VUEJS](https://img.shields.io/badge/vue-2.6.14-green.svg)](https://opensource.org/licenses/MIT)

![Alt text](images/main.png "How the tool look like")
Made with VueJs
## Setting your own coef

Just change the src/assets/coef.yaml file

## Run with Docker

```bash
# Build
docker build -t polytech_coef_simulator .
```

```bash
# Run
docker run -it polytech_coef_simulator:latest -p 8080:8080
```


## Dev setup
```
git clone git@github.com:Tomansion/Polytech-coef-simulator.git
```
```
cd Polytech-coef-simulator
```
```
npm install
```
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```
