# [![PongHub](static/band.png)](https://health.ch3nyang.top)

<div align="center">

🌏 [Live Demo](https://health.ch3nyang.top) | 📖 [English](README.md)

</div>

## 简介

PongHub 是一个开源的服务状态监控网站，旨在帮助用户监控和验证服务的可用性。它支持

- **🕵️ 零侵入监控** - 无需改动代码即可实现全功能监控
- **🚀 一键部署** - 通过 Actions 自动构建，一键部署至 Github Pages
- **🌐 全平台支持** - 兼容 OpenAI 等公共服务及私有化部署
- **🔍 多端口探测** - 单服务支持同时监控多个端口状态
- **🤖 智能响应验证** - 精准匹配状态码及正则表达式校验响应体
- **🛠️ 自定义请求引擎** - 自由配置请求头/体、超时和重试策略

![浏览器截图](static/browser_CN.png)

## 快速开始

1. Star 并 Fork [PongHub](https://github.com/WCY-dt/ponghub)

2. 修改根目录下的 [`config.yaml`](config.yaml) 文件，配置你的服务检查项

3. 修改根目录下的 [`CNAME`](CNAME) 文件，配置你的自定义域名

   > 如果你不需要自定义域名，请删除 `CNAME` 文件

4. 提交修改并推送到你的仓库，GitHub Actions 将自动更新，无需干预

> [!TIP]
> 默认情况下，GitHub Actions 每 30 分钟运行一次。如果你需要更改运行频率，请修改 [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) 文件中的 `cron` 表达式。
> 
> 请不要将频率设置过高，以免触发 GitHub 的限制。

> [!IMPORTANT]
> 如果 GitHub Actions 未正常自动触发，手动触发一次即可。

## 配置说明

配置文件 `config.yaml` 的格式如下：

| 字段              | 类型   | 描述                          | 必填 |
|-----------------|--------|-----------------------------|----|
| `timeout`       | 整数   | 每次请求的超时时间，单位为秒              | ✖️  |
| `retry`         | 整数   | 请求失败时的重试次数                  | ✖️  |
| `max_log_days`  | 整数   | 日志保留天数，超过此天数的日志将被删除         | ✖️  |
| `services`      | 数组   | 服务列表                        | ✔️  |
| `services.name` | 字符串 | 服务名称                        | ✔️  |
| `services.health` | 数组 | 健康检查配置列表                    | ✖️  |
| `services.health.url` | 字符串 | 检查的 URL                     | ✔️  |
| `services.health.method` | 字符串 | HTTP 方法（`GET`/`POST`/`PUT`） | ✖️  |
| `services.health.status_code` | 整数 | 期望的 HTTP 状态码（默认 `200`）        | ✖️  |
| `services.health.response_regex` | 字符串 | 响应体内容的正则表达式匹配               | ✖️  |
| `services.health.body` | 字符串 | 请求体内容，仅在 `POST` 请求时使用            | ✖️  |
| `services.api` | 数组 | API 检查配置列表，格式同上 | ✖️  |

下面是一个示例配置文件：

```yaml
timeout: 5
retry: 2
max_log_days: 30
services:
  - name: "GitHub API"
    health:
      - url: "https://api.github.com"
    api:
      - url: "https://api.github.com/repos/wcy-dt/ponghub"
        method: "GET"
        status_code: 200
        response_regex: "full_name"
  - name: "Ch3nyang's  Websites"
    health:
      - url: "https://example.com/health"
        response_regex: "status"
      - url: "https://example.com/status"
        method: "POST"
        body: '{"key": "value"}'
```

> [!NOTE]
> `health` 和 `api` 至少有一个。这两者在处理上没有区别，是为未来扩展做的预留。

## 免责声明

[PongHub](https://github.com/WCY-dt/ponghub) 仅用于个人学习和研究，不对程序的使用行为或结果负责。请勿将其用于商业用途或非法活动。
