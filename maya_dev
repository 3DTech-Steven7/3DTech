# coding = 'utf-8'
import numpy as np
from cython.parallel import prange 
#import pandas as pd
import pymel.core as pm
from collections import defaultdict
import time

def count_time(func):
    def int_time(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        over_time = time.time()
        total_time = over_time - start_time
        print('程序共计耗时%s秒' % total_time)
        return result

    return int_time

@count_time
def target_mean_v0(data):
    data_shape = len(data)
    result = dict()
    value_dict = defaultdict(int)
    count_dict = defaultdict(int)

    for i in range(data_shape):
        data_loc_x = int(data[i][0])
        data_loc_y = data[i][1]
        value_dict[data_loc_x] += data_loc_y
        count_dict[data_loc_x] += 1
        
    for i in range(data_shape):
        data_loc_x = int(data[i][0])
        data_loc_y = data[i][1]

        result[i] = (value_dict[data_loc_x] - data_loc_y) / count_dict[data_loc_x] 
    return result

@count_time
def target_mean_v1(data):
    data_shape = data.shape[1]
    result = np.zeros(data.shape[1])
    value_dict = defaultdict(int)
    count_dict = defaultdict(int)

    for i in range(data_shape):
        data_loc_x = int(data[0, i])
        data_loc_y = data[1, i]
        
        value_dict[data_loc_x] += data_loc_y
        count_dict[data_loc_x] += 1
        
    for i in range(data_shape):
        data_loc_x = int(data[0, i])
        data_loc_y = data[1, i]

        result[i] = (value_dict[data_loc_x] - data_loc_y) / count_dict[data_loc_x] 
    return result

if __name__ == '__main__':
	joint_list = pm.ls(typ="joint")
	joint_list = [(i.side.get(), i.radius.get()) for i in joint_list]
	joint_list*=1000

	joint_array = np.array(joint_list).T

	m=dict()
	for i,j in enumerate(target_mean_v1(joint_array).tolist()):
	   m[i] = j 

	m == target_mean_v0(joint_list)
