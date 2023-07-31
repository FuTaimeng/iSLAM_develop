# iSLAM: Imperative SLAM

iSLAM is a novel learning framework for SLAM tasks, which fosters reciprocal correction between the front-end and back-end, thus enhancing performance without necessitating any external supervision. 

<img src='docs/iSLAM anim.gif' width=500>

We formulate a SLAM system as a **bi-level optimization** problem to make the front-back-ends bidirectionally connected. As a result, the front-end model is able to learn global geometric knowledge obtained through pose graph optimization by back-propagating the residuals from the back-end. We call this **imperative learning** due to the passive nature of this process.


<img src='docs/bilevel.png' width=500>

This design significantly improves the generalization ability of the entire system and thus achieves the accuracy improvement up to 45%. To the best of our knowledge, iSLAM is the first SLAM system showing that the front-end and back-end can learn jointly and mutually contribute to each other in a self-supervised manner.

## Related Papers

**iSLAM: Imperative SLAM**, Taimeng Fu, Shaoshu Su, and Chen Wang. "" *arXiv preprint arXiv:2306.07894*, 2023. [PDF](https://arxiv.org/pdf/2306.07894.pdf).

Please cite us as:
```
@article{fu2023islam,
  title={iSLAM: Imperative SLAM},
  author={Fu, Taimeng and Su, Shaoshu and Wang, Chen},
  journal={arXiv preprint arXiv:2306.07894},
  year={2023}
}
```

## Experinemnt Results

We test our framework on [KITTI](https://www.cvlibs.net/datasets/kitti/) and [EuRoC](https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets) benchmarks, which have distinctive motion patterns. It is seen that after 6 iterations, both the front-end and back-end improve themselves significantly. Note that this is a self-learning process and no ground truth labels are used.

<img src='docs/improve.png' width=500>

The figure below visualizes the improvements VO trajectories through imperative learning.

<img src='docs/trajectory.png' width=800>

## More Info

Video: coming soon!

Webpage: coming soon!

## Run Code

### Create Conda Enviroment

Please use the `environment.yml` we provided.

```
conda env create -f environment.yml
```

### Prepare Data

#### KITTI

We tested our framework on KITTI Odometry. However, as KITTI Odometry does not contain IMU data, we downloaded [KITTI Raw](https://www.cvlibs.net/datasets/kitti/raw_data.php) instead. The mapping between the two is:

```
00: 2011_10_03_drive_0027 000000 004540
01: 2011_10_03_drive_0042 000000 001100
02: 2011_10_03_drive_0034 000000 004660
03: 2011_09_26_drive_0067 000000 000800
04: 2011_09_30_drive_0016 000000 000270
05: 2011_09_30_drive_0018 000000 002760
06: 2011_09_30_drive_0020 000000 001100
07: 2011_09_30_drive_0027 000000 001100
08: 2011_09_30_drive_0028 001100 005170
09: 2011_09_30_drive_0033 000000 001590
10: 2011_09_30_drive_0034 000000 001200
```

We use synchronized images and raw IMU data (as the synchronized IMU is only 10Hz, while the raw is 100Hz). So you need to download both and replace the `oxts` folder in the synchronized data with the one in the raw data. You can use `./tools/replace_imu.py` to do the replacement.

#### EuRoC

Please download EuRoC [here](https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets).

### Download Pretrain Model

Please download the pretrain model [here](https://buffalo.box.com/s/r4urjlumi6b7k6bf75c3l7karqg2846c) and put it in `./models`.

### Run Scripts

We provide `run_kitti.sh` and `run_euroc.sh`. Please open and change `data_dir` to your path to a specific sequence. The results will in `./train_results/${project_name}/${train_name}` and the trained models will in `./train_results_models/${project_name}/${train_name}`, so you may also change these two variables to specify the output path.

Finally, just run:

```
sh run_kitti.sh
sh run_euroc.sh
```

## Acknowledgements

This work was partially supported by Cisco Systems, Inc.