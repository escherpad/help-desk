# Escherpad Help Desk

This is a github repo used for discussions and support on Escherpad.

You can:

*   ask for new features,
*   file bug reports on existing features,
*   talk to each other
*   hit us with a stick if we break things.

We also have a gitter channel here:      [link to our channel](https://gitter.im/escherpad/help-desk)

## Requirement

We currently expect you to be familiar with the setup of a jupyter server.

Here at Escherpad, we want to make programming and computation more accessible for everybody, and our current technology stack allows us to do that with much ease. For now, since the team is just myself, I will focus on building the core product that power users are happy with. If you would like to help making it easier for learn python, julia, R or other languages for people who just started, you can help out by writing up setup tutorials. I am working on a blogging platform called `lesquare` to make sharing and collaborating on those things easier.

## On ~~iPython~~ Jupyter kernel server authorization

Regardless of the authentication scheme (of both jupyter and jupyterHub), the notebook server's api endpoint authorization is currently done using cookies. This is a out-dated authorization method. Modern browser usually prevent setting cookies to a domain different from the one that the webpage is served from for [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), which means a third-party web application like escherpad can not get authorized to the jupyter server you have such as `http://localhost:8888` or `http://your-uni.edu:8000` as long as the authorization is cookie based.

Supporting third-party clients connected to a self-hosted notebook server is a common usecase lots of us in physics have, so I opened up an issue on jupyter/notebook about this.

You can help by up-voting these github issues to let the jupyter/notebook team know that a more modern token-based authorization method is needed. I will add password protection as soon as json token based authorization is added to the notebook!

*   [https://github.com/jupyter/notebook/issues/325](https://github.com/jupyter/notebook/issues/325)
*   [https://github.com/jupyter/jupyterhub/issues/301](https://github.com/jupyter/jupyterhub/issues/301)

## Setting up jupyter server

It takes only two steps to setup your local jupyter server! Before you go through the setup, here is a short video of what Escherpad's real-time jupyter client is all about:

[![real-time collaborative iPython notebook demo](http://img.youtube.com/vi/si0QFaDStoo/maxresdefault.jpg)](http://www.youtube.com/watch?v=si0QFaDStoo)

Now you have watched the video. Let's set it up!

### Step 1: Install the jupyter notebook server

run:

```bash
sudo apt-get update
sudo apt-get install python3-dev python3-pip build-essential libzmq3-dev

pip install jupyter

```

you shouldn't need `sudo` below:

```bash
pip install ipython jupyter --upgrade

```

### Step 2: Configure your jupyter profile

to connect the Escherpad third-party jupyter client, you need to configure your jupyter kernel server to allow cross-origin http requests! The way to do this is the following:

1.  First setup a profile for iPython:

    ```bash
    jupyter notebook --generate-confi

    ```

    this should return us the path of the configuration file that ipython just created. For us it is the one below:

    ```bash
    writing default config to: /home/me/.jupyter/jupyter_notebook_config.py

    ```

    The useful thing is this file here:

    ```bash
    ~/.jupyter/jupyter_notebook_config.py

    ```

2.  Now you can go into the created profile config file and add the following lines:

    ```bash
    vim ~/.jupyter/jupyter_notebook_config.py

    ```

    and add

    ```python
    c.NotebookApp.allow_origin = "http://www.escherpad.com"
    c.NotebookApp.ip = "*"
    # The following line is optional. 
    # It specifies the port the server runs from.
    # this has to be a int not a string
    c.NotebookApp.port = 8000

    ```

3.  Now you can fire up the jupyter (ipython notebook) server:

    ```bash
    jupyter notebook --config=~/.jupyter/jupyter_notebook_config.py

    ```

    what I noticed is that this sometimes doesn't work on windows machines. Since escherpad is about collaboration, please comment to the side or suggest updates to help correct this!

    the default port for jupyter server is `8888`. So if the server you just fired up uses port `8000` then it means our configuration file has worked!

### Finally:

to connect one of your escherpad notebooks with your jupyter server,

1.  first create a new note

    ![](http://www.escherpad.com/res/notes/55ebd0784e40168e500aff25/images/7f4a16c192f615be3f841894716f46a0-screenshot_2015-09-16_01_17_24.png)

2.  type a note title you want, and then add `.ipynb` extension to it.

    ![](http://www.escherpad.com/res/notes/55ebd0784e40168e500aff25/images/155bee079e615a538f479e7013929881-screenshot_2015-09-16_01_17_57.png)

    now the notebook view should change to a ipython notebook view, and if you have any content in the original note, the html content would've been converted to markdown.

    ![](http://www.escherpad.com/res/notes/55ebd0784e40168e500aff25/images/6f39f0e87b64b220c66ca3eca2bcdbb3-screenshot_2015-09-16_01_18_07.png)

3.  Now to connect to a kernel, click on the kernel status button, a popover should appear. Click on the add kernel item, a modal should appear.

    ![](http://www.escherpad.com/res/notes/55ebd0784e40168e500aff25/images/52ccc569bf50c38f7bfa8a48e337a930-screenshot_2015-09-16_01_18_33.png)

    ![](http://www.escherpad.com/res/notes/55ebd0784e40168e500aff25/images/b229817bf7c9cb6bb3e5e48e5ebc73c5-screenshot_2015-09-16_01_18_44.png)

4.  In this modal, type in the full base url including the port number. Do NOT include the `/tree` route. The directory name is a virtual folder that is used to form part of the ID for that note. After you finish, click on the connect to kernel button.

    ![](http://www.escherpad.com/res/notes/55ebd0784e40168e500aff25/images/708894479014470f4a2f51e0d68702d7-screenshot_2015-09-16_01_19_44.png)

5.  Now close the modal, the kernel should show "idle", and you are ready to go!

    ![](http://www.escherpad.com/res/notes/55ebd0784e40168e500aff25/images/e62316e72dfe5dce95a6494964246bcf-screenshot_2015-09-16_01_19_58.png)

6.  Example notebook:

    ![](http://www.escherpad.com/res/notes/55ebd0784e40168e500aff25/images/710a1930dc0a4858e9deab689d5269e1-screenshot_2015-09-16_01_23_07.png)
