# Prometheus + Node Exporter + Grafana 监控系统搭建

本项目提供了一套完整的 **Prometheus + Node Exporter + Grafana** 服务器监控系统的搭建教程，适用于 Ubuntu 服务器。

## **1. 安装 Prometheus**

### **1.1 下载并安装 Prometheus**
```bash
# 下载 Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.34.0/prometheus-2.34.0.linux-amd64.tar.gz

# 解压文件
tar -xvzf prometheus-2.34.0.linux-amd64.tar.gz

# 进入解压后的目录
cd prometheus-2.34.0.linux-amd64
```

### **1.2 配置 Prometheus**
创建 `prometheus.yml` 配置文件，确保 Prometheus 采集 Node Exporter 数据。

```yaml
# prometheus.yml

global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']  # Node Exporter 默认端口
```

### **1.3 启动 Prometheus**
```bash
./prometheus --config.file=prometheus.yml &
```
默认 Web UI 访问地址：`http://localhost:9090`

---

## **2. 安装 Node Exporter**

### **2.1 下载并安装 Node Exporter**
```bash
# 下载 Node Exporter
wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz

# 解压文件
tar -xvzf node_exporter-1.3.1.linux-amd64.tar.gz

# 进入解压后的目录
cd node_exporter-1.3.1.linux-amd64
```

### **2.2 启动 Node Exporter**
```bash
./node_exporter &
```

默认 Web 访问地址：`http://localhost:9100/metrics`

---

## **3. 安装 Grafana**

### **3.1 安装 Grafana**
```bash
# 添加 Grafana 仓库
sudo apt-get install -y software-properties-common
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee /etc/apt/sources.list.d/grafana.list

# 导入 Grafana GPG 密钥
sudo apt-get install -y gnupg
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7F0CEB10

# 更新并安装 Grafana
sudo apt-get update
sudo apt-get install grafana
```

### **3.2 启动 Grafana**
```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```
默认 Web 访问地址：`http://localhost:3000`
- 默认用户名：`admin`
- 默认密码：`admin`

---

## **4. 在 Grafana 配置 Prometheus 数据源**

1. 登录 Grafana Web UI：`http://localhost:3000`
2. 点击 `⚙️ Configuration` -> `Data Sources`
3. 选择 `Add data source`，选择 **Prometheus**
4. 在 `URL` 字段中输入 Prometheus 地址：
   ```
   http://localhost:9090
   ```
5. 点击 **Save & Test**，确保连接成功

---

## **5. 在 Grafana 创建仪表盘**

### **5.1 使用 Grafana 官方模板**
1. 在 Grafana 中，点击 `+` -> `Import`
2. 在 `Import via grafana.com` 字段输入模板 ID：
   ```
   1860  # Node Exporter 监控模板
   ```
3. 点击 **Load**，选择 Prometheus 作为数据源，点击 **Import**

---

## **6. 运行和管理**

### **6.1 验证运行状态**
```bash
# 检查 Prometheus 是否运行
ps aux | grep prometheus

# 检查 Node Exporter 是否运行
ps aux | grep node_exporter

# 检查 Grafana 是否运行
systemctl status grafana-server
```

### **6.2 停止服务**
```bash
pkill prometheus
pkill node_exporter
sudo systemctl stop grafana-server
```

### **6.3 开机自启动（可选）**
```bash
sudo systemctl enable grafana-server
```

---

## **总结**
| 步骤 | 组件 | 作用 |
|------|------|------|
| 1 | Prometheus | 监控数据收集器 |
| 2 | Node Exporter | 采集服务器硬件数据（CPU、内存、磁盘） |
| 3 | Grafana | 数据可视化（仪表盘） |

你现在可以在 **Grafana** 中查看服务器的 CPU、内存、磁盘使用情况，搭建一个强大的监控系统！ 🚀
