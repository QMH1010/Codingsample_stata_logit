# Codingsample_stata_logit
#logit regression between 3 IVs AND 3DVs respectively
# Load the dataset (make sure you replace the path to your dataset)
use "D:\0_stata15_数据分析资料\wvs_china_vietnam_2012-2017\WVS_Wave_7_Vietnam_Stata_v5.0.dta", clear  
# 开始时保存当前数据的状态
preserve

# 2. Check the data structure
describe       
list in 1/10  

# 3. Generate new variables
ren Q4 v1
ren Q288R v2
ren Q257 v3
# demographical variable
ren Q275 education    
# demographical variable
ren Q262 age      
# continuous dependent variable    
ren Q254 support1  
 # continuous dependent variable   
ren Q221 support2    
 # Binary variable
ren Q71 support3     
# check for missing data
count if !missing(support1) 
count if !missing(support2) 
count if !missing(support3)       
 # check for data distribution
histogram support1 
histogram support2
histogram support3               

#4.Generate treatment # 假设 feel close to the country 大于 2的为实验组，其他为对照组（有条件的实验组）
gen treatment = (v3>2) 
tabulate treatment

#5.描述性统计
summarize support1 treatment, detail

#6.t值检验
ttest support1, by(treatment)

#7. 有多个控制变量，进行回归分析
regress support1 treatment education age

#8. 查看回归结果
estimates table

#恢复之前的数据状态
restore

