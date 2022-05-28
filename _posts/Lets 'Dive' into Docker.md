![[wallpaperflare.com_wallpaper.jpg]]

Hi Everyone,

In this post we are going to see about docker optimization tool called **Dive**.  A tool for exploring docker image layer by layer and find a way to optimize your docker image size. We all knew docker image build top of the different layers and more layer you add on the more spaces will taken up by the image.so, more layer in the image then more space the image will require.

In other words, each line you add in the docker file add new layer in the image, Here is an example for docker layer (credits : docker official)  you can see the docker file has six layers.

```bash
# syntax=docker/dockerfile:1
FROM node:12-alpine #this one has own no of layers
RUN apk add --no-cache python2 g++ make #layer1
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000 #layer6
```

##### What is Dive???
Dive is a opensource tool developed by *Alex Goodman*. It's used for Analyzing docker image contents layer by layer. It breaks down image by layer and show contents of the each layer then shows the potentially wasted space in the image and efficiency score of the image.

##### How to Install???
**Ubuntu/Debian**

```bash
wget https://github.com/wagoodman/dive/releases/download/v0.9.2/dive_0.9.2_linux_amd64.deb
sudo apt install ./dive_0.9.2_linux_amd64.deb
```
**RHEL/Centos**
```bash
curl -OL https://github.com/wagoodman/dive/releases/download/v0.9.2/dive_0.9.2_linux_amd64.rpm
rpm -i dive_0.9.2_linux_amd64.rpm
```
**Arch Linux**
```bash
Available as [dive](https://aur.archlinux.org/packages/dive/) in the Arch User Repository (AUR).

yay -S dive

The above example assumes [`yay`](https://aur.archlinux.org/packages/yay/) as the tool for installing AUR packages.
```
**Mac**
```bash
If you use [Homebrew](https://brew.sh):

brew install dive

#If you use (https://www.macports.org):
sudo port install dive
```

*There are some other ways to install this one, check out if you want : *
https://github.com/wagoodman/dive

##### Dive usage :
1. Goto terminal just type `
```bash 
$ dive <image name or id>
```

2. If you want to build an image then analyze use 
```bash
$ dive build -t <image:tag>
			or
$ dive build -f '<path to docker file>'
```

3. In my case I'm gonna use Drozer image from fsecurelabs
```shell
$ dive fsecurelabs/drozer                                                       
Image Source: docker://fsecurelabs/drozer
Fetching image... (this can take a while for large images)
Analyzing image...
Building cache...
```

![[Pasted image 20220528123917.png]]

As you can see this image has seven layers and dive give us detailed information about each layer. Dive has four section start from the top we could see Layers follwed by layer details, image details and finally current layer contents.
- Layers: In this panel shows layer size and the command we include in the docker file.

- Layer Details: This panel contains layer ID and hash and command used in the layer.

![[Pasted image 20220528134247.png]]

- Current layer contents: It contains the files of the each layer, you can use arrow keys and tab key to navigate with them. While scrolling you can see some files in **green**,**yellow** and **red** color. Green indicates newly added files and Orange indicated modified files and Red means Deleted files in the layer.

-   Image details: This includes Image id, Image size and most important *potentially wasted space* and Image efficiency score.

##### Alright let's go the distance, How can we keep our docker image slim and good..
There are some ways to shrink your docker image
###### 1. Use small Images as much as possible (like alpine linux).
- Alpine image is very small less than 10MB and easy to use.

###### 2. Use .dockerignore file.
- Similar to .gitignore file that exclude unnecessary files for your image.

###### 3. Avoid multiple layering.
- Lets take an Example

```bash
RUN apt-get update

RUN apt-get -y install wget

RUN apt-get -y install curl

RUN wget https://github.com/wagoodman/dive/.../v0.9.2/dive_0.9.2_linux_amd64.deb

RUN dpkg -i dive_0.9.2_linux_amd64.deb

RUN apt-get install firefox

RUN apt-get install chrome
```

In above docker file you can see there are seven lines so that means we have seven layers. so, abviously it occupies more spaces. In order to reduce this we could use following

```bash
RUN apt-get update \
&& RUN apt-get -y install wget \
&& RUN apt-get -y install curl

RUN wget https://github.com/wagoodman/dive/.../v0.9.2/dive_0.9.2_linux_amd64.deb

RUN dpkg -i dive_0.9.2_linux_amd64.deb

RUN apt-get install firefox \
&& RUN apt-get install chrome
```

So we reduced docker file from seven to four lines, this will save some space in your docer image.

And thats all, Thanks for Reading... <3