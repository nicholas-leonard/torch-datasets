# Datasets

A collection of easy to use datasets for training and testing machine learning
algorithms with Torch7.

## Install

    Dependencies:

    git clone https://github.com/rosejn/lua-util.git
    git clone https://github.com/rosejn/lua-fn.git
    git clone https://github.com/rosejn/lua-pprint.git
    
    You will also need to make the above acessible in the LUA_PATH 
    environement variable. Example .bashrc file:
    
    export DEV_PATH=/home/username/projects
    export LUA_PATH=";;"$DEV_PATH"/lua-pprint/?/init.lua;"
    $DEV_PATH"/lua-util/?.lua;"$DEV_PATH"/lua-util/?/init.lua;"
    $DEV_PATH"/lua-fn/?.lua;"$DEV_PATH"/lua    -fn/?/init.lua;"
    $DEV_PATH"/torch-datasets/?.lua;"$DEV_PATH"/torch-datasets/?/init.lua;"
    
    Install these lua rocks (packages):
    
    sudo luarocks install unsup
    sudo luarocks install fs
    
    Set the dataset directory in .bashrc file:
    
    export TORCH_DATA_PATH=/var/lib/torch

## Usage

    require('dataset/mnist')
    m = Mnist.dataset()
    m:size()                      -- => 60000
    m:sample(100)                 -- => {data = tensor, class = label}

    -- scale values between [0,1] (by default they are in the range [0,255])
    m = Mnist.dataset({scale = {0, 1}})

    -- or normalize (subtract mean and divide by std)
    m = Mnist.dataset({normalize = true})

    -- only import a subset of the data (imports full 60,000 samples otherwise),
    -- sorted by class label
    m = Mnist.dataset({size = 1000, sort = true})


To process a randomly shuffled ordering of the dataset:

    for sample in m:sampler() do
      net:forward(sample.data)
    end


Or access mini batches:

    local batch = m:mini_batch(1)

    -- or use directly
    net:forward(m:mini_batch(1).data)

    -- set the batch size using an options table
    local batch = m:mini_batch(1, {size = 100})


To process the full dataset in randomly shuffled mini-batches:

    for batch in m:mini_batches() do
       net:forward(batch.data)
    end


Generate animations over 10 frames for each sample, which will
randomly rotate, translate, and/or zoom within the ranges passed.

    local anim_options = {
        frames      = 10,
        rotation    = {-20, 20},
        translation = {-5, 5, -5, 5},
        zoom        = {0.6, 1.4}
     }
     s = dataset:sampler({animate = anim_options})


Standard pipeline options can be used to add post-processing stages (e.g. binarize and flatten):

     s = dataset:sampler({pad = 5, binarize = true, flatten = true})


Pass a custom pipeline for processing samples:

     s = dataset:sampler({pipeline = my_pipeline})


Create a dataset from bunch of images in a directory

     require 'datset/imageset'
     d = ImageSet.dataset({dir='your-data-directory'})
     while true do w=image.display({image=d().data,win=w}) util.sleep(1/10) end

Create a dataset from bunch of videos in a directory

     require 'datset/videoset'
     d = VideoSet.dataset({dir='KTH'})
     while true do w=image.display({image=d().data,win=w}) util.sleep(1/10) end


