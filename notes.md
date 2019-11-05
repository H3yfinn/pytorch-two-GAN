-Hoping I don't need to do anything about the webpage utility. I just want it to train over the weekend and I'll hope for the best.
-Make sure to include: --gpu_ids -1 at the end of train and test scripts because we dont have a gpu on this jupyterlab system (yet). do it like this: 
python train_two_pix2pix.py --dataroot ./datasets/soccer_seg_detection --name soccer_seg_detection_pix2pix --model two_pix2pix --which_model_netG unet_256 --which_direction AtoB --lambda_A 100 --dataset_mode two_aligned --no_lsgan --norm batch --pool_size 0 --output_nc 1 --phase1 train_phase_1 --phase2 train_phase_2 --save_epoch_freq 2 --gpu_ids -1

----------

python test_two_pix2pix.py --dataroot ./datasets/soccer_seg_detection \
--which_direction AtoB --model two_pix2pix --name soccer_seg_detection_pix2pix \
--output_nc 1 --dataset_mode aligned --which_model_netG unet_256 --norm batch --how_many 186 --loadSize 2 --gpu_ids -1

-With the above what do all the variables mean?
-how_many seems to dictate how many images are put through the model
-loadSize... im not too sure tbh. I couldn't find it anywhere (had a short look. GUess it's not too important)
-model is an important variable. Seems like this functionality for football fields uses two_pix2pix. (I'll read up on this later) but also:
-dataset_mode seems to be quite important for the actual pix2pix model. If using 2pix2pix then it must be set to aligned (validation and testing) or two_aligned (training). if set to unaligned then it should be using CycleGANModel (which i think may be part of the training phase).
-check results/test_latest/images for the results. If I test with how_many set to 1 then the files 001_AB-checkpoint_fake_B,C,D.jpg and 001_AB-checkpoint_real_A,D.jpg will be updated. Why those letters? And how to implement them in the next step, sccvsd?
-----------

-It worked. Now to link this process in a pipe with the sccvsd.
First use this:
python test_two_pix2pix.py --dataroot ./datasets/soccer_seg_detection --which_direction AtoB --model two_pix2pix --name soccer_seg_detection_pix2pix --output_nc 1 --dataset_mode aligned --which_model_netG unet_256 --norm batch --how_many 1 --loadSize 256 --gpu_ids -1

Then for sccvsd it gets a bit tricky. Need to find a way of isolating the input image projected on template. Can be done lah
