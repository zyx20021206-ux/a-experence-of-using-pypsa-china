# 本文件以说明在一台新电脑上安装pypsa-china模型的详细步骤
# 官方教程也是有的，但是主包研究了很久是需要做一些调整的，官方网站 https://pik-piam.github.io/PyPSA-China-PIK/1.3.2/

# 首先安装conda环境，本系统使用anaconda为例

# 根据本文件夹下的environment_combined.yaml文件创建虚拟环境（这个文件夹是主包根据官方给的两个yaml文件结合而成
# 如果后续出现package版本问题可以尝试官方给的yaml文件，尤其是linux系统运行建议使用官方文件）
# 正常的win10/11用我的应该不会有问题
# 环境创建具体命令因人而异，可参考本人命令行
conda env create -f C:\Users\Administrator\Desktop\environment_combined.yaml -p I:\pypsa-china

# 创建虚拟环境后在GitHub拉取pik的pypsa-china模型，也可以手动下载    网址 https://github.com/pik-piam/PyPSA-China-PIK
# 解压后进入文件夹PyPSA-China-PIK-main
cd I:\PyPSA-China-PIK-main
# 激活环境
conda activate I:\pypsa-china

# 测试文件需要去网站下载4个文件，大概十几个g，放在本工程下的data文件夹下
# 下载失败的话可以去网站下，国内下载非常慢 网址 https://zenodo.org/records/17719794
# 共四个文件，需放在./resources/cutouts中

# 此时进行干运行应该就通过了
snakemake -n
# 这是正式运行前的演练，主包一边运行一边编写本文件没有出现问题，如果出现问题可以联系主包或者直接ai
# 反馈大概是这样的就可以
This was a dry-run (flag -n). The order of jobs does not reflect the order of execution.

# 最后一步需要求解器许可，官方提供了开源求解器的选项，但是因为主包有在读身份，申请了一个Gurobi学术许可，发邮件有点麻烦，但是回的很快
# Gurobi的激活根据邮件进行即可

# 通过后可以尝试正式运行
snakemake or snakemake --cores 4  # 这取决于你的电脑性能
# 需注意这一步会进行完整的模型构建，其中要下载土地利用 / 地形数据等数据，这一步极易因网络问题而失败
# 失败了科学上网可以多下几遍，或者直接手动下载

# 或者你也可以按照官方的测试来做，这取决于你的习惯
conda activate pypsa-china
cd <workflow_location>
pytest tests/integration

# 此时用windows的同学应该会迎来第一次报错
WorkflowError:
At least one job did not complete successfully.
# 大概率是因为Windows系统跟其他系统的语法问题，把报错和出错文件一起丢给ai即可
# 我的出错一般是需要把单引号改成双引号

# 到此全部配置就完成了，模型的训练需要一点时间，可以设置虚拟内存以保护电脑