# Basic Commands for Docker


## List and view images:

List all images:
	```sh
	sudo docker image ls --all
	```


## Delete images/containers

Remove the whole docker system:
	```sh
	sudo docker system prune
	```

We can also remove just images, containers, or volumes:
	```sh
	sudo docker image prune
	sudo docker container prune
	sudo docker volume prune
	```

We can just delete one image by name:
	```sh
	sudo docker rmi -f image_name
	```

## Run docker image and mount a folder

In some cases, it is useful to mount a host folder to share data with docker image.
	```sh
	docker run -it --rm -v /path/on/host:/data image_name
	```

