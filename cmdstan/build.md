# Building CMDSTAN from source


## Create docker image to build cmdstan

1. Build the image based on based Ubuntu 20.04
	```sh
	docker build -t cmdstan:latest .
	```

2. Make sure the following folder being created before running the image
	`/home/hanh/Docker`

3. Run the image to test
	```sh
	docker run -it --rm -v /home/hanh/Docker:/data cmdstan
	```

## Clone and build

1. Clone cmdstan repository (including all submodules):
	```sh
	git clone --recursive https://github.com/stan-dev/cmdstan.git
	```

2. Change dir and make
	```sh
	cd cmdstan
	make build
	```

3. Download prophet.stan and build
	```sh
	wget -P examples https://raw.githubusercontent.com/facebook/prophet/main/python/stan/prophet.stan
	```
	
	Now convert stan to cpp:
	```sh
	./bin/stanc  --o=examples/prophet.hpp examples/prophet.stan
	```
	
	Compile c++ source:
	```sh
	g++ -std=c++1y -pthread -D_REENTRANT -Wno-sign-compare -Wno-ignored-attributes      -I stan/lib/stan_math/lib/tbb_2020.3/include    -O3 -I src -I stan/src -I stan/lib/rapidjson_1.1.0/ -I lib/CLI11-1.9.1/ -I stan/lib/stan_math/ -I stan/lib/stan_math/lib/eigen_3.4.0 -I stan/lib/stan_math/lib/boost_1.84.0 -I stan/lib/stan_math/lib/sundials_6.1.1/include -I stan/lib/stan_math/lib/sundials_6.1.1/src/sundials    -DBOOST_DISABLE_ASSERTS          -c -Wno-ignored-attributes   -x c++ -o examples/prophet.o examples/prophet.hpp
	```
	
	Dynamic linking to binary:
	```sh
	cp -r stan/lib/stan_math/lib/tbb ./
	```
	
	```sh
	g++ -std=c++1y -pthread -D_REENTRANT -Wno-sign-compare -Wno-ignored-attributes      -I stan/lib/stan_math/lib/tbb_2020.3/include    -O3 -I src -I stan/src -I stan/lib/rapidjson_1.1.0/ -I lib/CLI11-1.9.1/ -I stan/lib/stan_math/ -I stan/lib/stan_math/lib/eigen_3.4.0 -I stan/lib/stan_math/lib/boost_1.84.0 -I stan/lib/stan_math/lib/sundials_6.1.1/include -I stan/lib/stan_math/lib/sundials_6.1.1/src/sundials    -DBOOST_DISABLE_ASSERTS               -Wl,-L,tbb -Wl,-rpath,tbb        examples/prophet.o src/cmdstan/main.o       -ltbb   stan/lib/stan_math/lib/sundials_6.1.1/lib/libsundials_nvecserial.a stan/lib/stan_math/lib/sundials_6.1.1/lib/libsundials_cvodes.a stan/lib/stan_math/lib/sundials_6.1.1/lib/libsundials_idas.a stan/lib/stan_math/lib/sundials_6.1.1/lib/libsundials_kinsol.a  stan/lib/stan_math/lib/tbb/libtbb.so.2 -o prophet
	```
	
	Packing the binary and libs:
	```sh
	mkdir -p dist/tbb
	cp prophet dist/
	cp tbb/libtbb.so.2 dist/tbb
	```
