## 1.怎样查看一年内缺失的wrfout文件
从已有的wrfout名的txt文本中，如文件内容格式如下：
```
wrfout_d01_2016-01-01_12_00_00
wrfout_d01_2016-01-01_13_00_00
wrfout_d01_2016-01-01_14_00_00
wrfout_d01_2016-01-01_15_00_00
wrfout_d01_2016-01-01_16_00_00
wrfout_d01_2016-01-01_17_00_00
```
目标是读取这个文件，找出从时间从2016-01-01_00_00_00到2017-01-01_00_00之间缺失的文件名
## 2.代码如下：
```
import pandas as pd
from datetime import datetime, timedelta

# 读取文件名并解析出时间
def parse_filenames(file_path):
    with open(file_path, 'r') as f:
        lines = f.read().splitlines()

    timestamps = []
    for line in lines:
        try:
            # 提取时间部分并转换为正确的格式
            # timestamp_str = line.split('_')[-3] + '_' + line.split('_')[-2] + '_' + line.split('_')[-1]
            timestamp_str= line[11:] 
            # print(timestamp_str)
            timestamp = pd.to_datetime(timestamp_str, format='%Y-%m-%d_%H_%M_%S')
            # print(timestamp)
            timestamps.append(timestamp)
        except ValueError as e:
            print(f"解析时间时出错，跳过该行: {line}, 错误: {e}")
    
    return pd.DatetimeIndex(timestamps)

# 生成完整的时间序列
def generate_full_timestamps(start, end, freq='H'):
    start = start.replace('_', ' ', 1).replace('_', ':', 2)
    end = end.replace('_', ' ', 1).replace('_', ':', 2)
    return pd.date_range(start=start, end=end, freq=freq)

# 找出缺失的时间
def find_missing_files(existing_timestamps, full_timestamps):
    missing_timestamps = full_timestamps.difference(existing_timestamps)
    missing_files = ['wrfout_d01_' + ts.strftime('%Y-%m-%d_%H_%M_%S') for ts in missing_timestamps]
    return missing_files

# 主程序
def main(file_path):
    start_time = '2016-01-01_00_00_00'
    end_time = '2017-01-01_00_00_00'
    
    # 解析已有文件的时间戳
    existing_timestamps = parse_filenames(file_path)
    
    # 生成完整时间序列
    full_timestamps = generate_full_timestamps(start=start_time, end=end_time)
    
    # 找出缺失的文件
    missing_files = find_missing_files(existing_timestamps, full_timestamps)
    
    if missing_files:
        print("缺失的文件名如下：")
        for file in missing_files:
            print(file)
    else:
        print("没有缺失的文件。")


# 使用路径调用程序
file_path = 'files_wrfout_2016.txt'  # 替换为实际路径
main(file_path)
```
## 3. 运行结果如下：
```
缺失的文件名如下：
wrfout_d01_2016-05-11_07_00_00
wrfout_d01_2016-05-11_08_00_00
wrfout_d01_2016-05-11_09_00_00
wrfout_d01_2016-05-11_10_00_00
wrfout_d01_2016-05-11_11_00_00
wrfout_d01_2016-05-11_12_00_00
wrfout_d01_2016-05-11_13_00_00
wrfout_d01_2016-05-11_14_00_00
wrfout_d01_2016-05-11_15_00_00
wrfout_d01_2016-05-11_16_00_00
wrfout_d01_2016-05-11_17_00_00
wrfout_d01_2016-05-11_18_00_00
wrfout_d01_2016-05-11_19_00_00
wrfout_d01_2016-05-11_20_00_00
wrfout_d01_2016-05-11_21_00_00
wrfout_d01_2016-05-11_22_00_00
wrfout_d01_2016-05-11_23_00_00
wrfout_d01_2016-08-09_01_00_00
wrfout_d01_2017-01-01_00_00_00
```