## 下载系列视频

```bash
mamba run -n yt_dlp_enve yt-dlp \
    -U \
    --cookies "/mnt/d/Download/chrome download/www.bilibili.com_cookies.txt" \
    --download-archive "bilibili_archive. txt" \
    -f "bv*+ba/b" \
    --playlist-items "1: " \
    -o "/mnt/f/媒体库/番剧/%(playlist)s/%(title)s.%(ext) s" \
    --downloader "aria2c" \
    --downloader-args "aria2c:-j5 -x16" \
    "https://www.bilibili.com/bangumi/play/ss53"
```

```bash
#!/bin/bash

# --- 脚本头部设置 ---

set -e
# -e: 如果命令返回非零退出状态（表示失败），脚本立即退出。
set -u
# -u: 引用未设置的变量时，脚本会报错并退出。
set -o pipefail
# -o pipefail: 如果管道中的任何命令失败，整个管道的退出状态将是该失败命令的退出状态。

# --- 代理清除函数 ---
function proxy_off() {
    unset http_proxy
    unset https_proxy
    unset no_proxy
    unset all_proxy
    unset HTTP_PROXY
    unset HTTPS_PROXY
    unset NO_PROXY
    echo "❌ 脚本内部: 代理已清除"
}

# --- 定义默认值 ---
# 默认cookie地址
DEFAULT_COOKIE_PATH="/mnt/d/Download/chrome download/www.bilibili.com_cookies.txt"
# 默认下载地址
DEFAULT_DOWNLOAD_BASE_PATH="/mnt/f/媒体库/番剧"
# 地址后缀
DEFAULT_OUTPUT_TEMPLATE_SUFFIX="/%(playlist)s/%(title)s.%(ext)s"

# --- 脚本内容 ---

echo "---下载系列视频设置---"
echo ""

# 1.清除代理环境变量
proxy_off

# 2.获取番剧网址(必须输入)
read -r -p "请输入番剧或视频网址 (例如: https://www.bilibili.com/bangumi/play/ss53): " DOWNLOAD_URL

if [ -z "$DOWNLOAD_URL" ]; then
    echo "错误: 网址不能为空, 脚本推出. "
    exit 1
fi

# 3.获取Cookie文件地址(可修改默认值)
echo ""
read -r -p "请输入Cookie文件路径 [默认: ${DEFAULT_COOKIE_PATH}]: " USER_COOKIE_PATH

COOKIE_PATH="${USER_COOKIE_PATH:-$DEFAULT_COOKIE_PATH}" # 如果USER_COOKIE_PATH为空, 则使用DEFAULT_COOKIE_PATH

# 4. 获取下载根目录(可修改根目录)
echo ""
read -r -p "请输入下载根目录 [默认: ${DEFAULT_DOWNLOAD_BASE_PATH}]: " USER_DOWNLOAD_BASH_PATH

DOWNLOAD_BASE_PATH="${USER_DOWNLOAD_BASH_PATH:-$DEFAULT_DOWNLOAD_BASE_PATH}"

# 构造最后的输出模板

OUTPUT_TEMPLATE="${DOWNLOAD_BASE_PATH}${DEFAULT_OUTPUT_TEMPLATE_SUFFIX}"

echo ""
echo "--- 下载任务详情 ---"
echo "目标网址: ${DOWNLOAD_URL}"
echo "Cookie地址: ${COOKIE_PATH}"
echo "下载路径: ${OUTPUT_TEMPLATE}"
echo "--------------------"
echo ""

read -r -p "按回车键开始下载，按Ctrl+C取消..." _ # _ 变量用于接收回车，不影响任何东西
echo ""                              # 打印空行，确保“开始下载”前有间隔

echo "开始下载"

mamba run -n yt_dlp_enve yt-dlp \
    -U \
    --cookies "${COOKIE_PATH}" \
    --download-archive "bilibili_archive.txt" \
    -f "bv*+ba/b" \
    --playlist-items "1:" \
    -o "${OUTPUT_TEMPLATE}" \
    --downloader "aria2c" \
    --downloader-args "aria2c:-j5 -x16" \
    "${DOWNLOAD_URL}"

echo "下载结束"

```

## 下载音频

```bash
mamba run -n yt_dlp_enve yt-dlp \
    -U \
    --cookies "/mnt/d/Download/chrome download/www.bilibili.com_cookies.txt" \
    --download-archive "bilibili_audio_archive.txt" \
    -f "ba" \
    --extract-audio \
    --audio-format mp3 \
    --playlist-items "1:" \
    -o "/mnt/c/Users/Sophomores/Desktop/音乐/%(title)s.%(ext)s" \
    --downloader "aria2c" \
    --downloader-args "aria2c:-j5 -x16" \
    "https://www.bilibili.com/video/BV1TE411n7um/"
```


```bash
mamba run -n yt_dlp_enve yt-dlp \
    -U \
    -x \
    --download-archive "asmrgay_audio_archive.txt" \
    --audio-format mp3 \
    -o "/mnt/c/Users/Sophomores/Desktop/%(title)s.%(ext)s" \
    --downloader "aria2c" \
    --downloader-args "aria2c:-j5 -x16" \
    
```

aria2c \
    --input-file="/mnt/d/Download/chrome download/桥桥超温柔.m3u8" \
    --dir="/mnt/c/Users/Sophomores/Desktop/桥桥超温柔/" \
    --max-concurrent-downloads=5 \
    --max-connection-per-server=16


## 下载视频和音频

```bash
#!/bin/bash

# --- 配置区 ---
COOKIES_FILE="/mnt/e/Chrome Download/www.bilibili.com_cookies.txt"
VIDEO_ROOT_DIR="/mnt/f/媒体库/B站音视频/视频" # 所有下载的视频将放在此目录下的播放列表子目录中
AUDIO_ROOT_DIR="/mnt/f/媒体库/B站音视频/音频" # 所有提取的音频将放在此目录下的播放列表子目录中

# 要下载的B站URL列表
URLS=(
  "https://space.bilibili.com/67815486/lists/290421"
  "https://space.bilibili.com/6065166/lists/1129502"
  "https://www.bilibili.com/video/BV1F64y1m74E/"
  "https://www.bilibili.com/video/BV13z4y1o78c"
  "https://www.bilibili.com/video/BV1aJ411S7sJ/"
  "https://www.bilibili.com/video/BV1sB4y1G7zm/"
  "https://www.bilibili.com/video/BV1i7411v7PF/"
  "https://www.bilibili.com/video/BV1bo4y1377c/"
  "https://www.bilibili.com/video/BV17b421J7bW/"
)

# --- 检查依赖 (保持不变) ---
if ! command -v mamba &> /dev/null; then
    echo "错误：mamba 未安装或不在 PATH 中。"
    exit 1
fi
if ! command -v ffmpeg &> /dev/null; then
    echo "错误：ffmpeg 未安装或不在 PATH 中。请安装 ffmpeg。"
    exit 1
fi

# --- 阶段 1: 使用 yt-dlp 下载视频 (保持不变) ---
echo "--- 阶段 1: 开始下载视频 ---"
YT_DLP_OUTPUT_TEMPLATE="${VIDEO_ROOT_DIR}/%(playlist)s/%(title)s.%(ext)s"

mamba run -n yt_dlp_enve yt-dlp \
  -U \
  --cookies "${COOKIES_FILE}" \
  --download-archive "bilibili_archive.txt" \
  -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best" \
  --output "${YT_DLP_OUTPUT_TEMPLATE}" \
  --downloader "aria2c" \
  --downloader-args "aria2c:-j5 -x16" \
  "${URLS[@]}"

```

批量提取音频
```bash
shopt -s globstar; \
VIDEO_ROOT_DIR="/mnt/f/媒体库/B站音视频/视频"; \
AUDIO_ROOT_DIR="/mnt/f/媒体库/B站音视频/音频"; \
for video_full_path in "${VIDEO_ROOT_DIR}"/**/*.mp4; do \
  if [ ! -f "${video_full_path}" ]; then continue; fi; \
  relative_to_video_root="${video_full_path#${VIDEO_ROOT_DIR}/}"; \
  base_name_no_ext="${relative_to_video_root%.mp4}"; \
  output_audio_path="${AUDIO_ROOT_DIR}/${base_name_no_ext}.mp3"; \
  mkdir -p "$(dirname "${output_audio_path}")"; \
  echo "处理文件: ${video_full_path}"; \
  echo "输出到: ${output_audio_path}"; \
  ffmpeg -i "${video_full_path}" -vn -c:a libmp3lame -q:a 0 -y -hide_banner "${output_audio_path}"; \
  if [ $? -ne 0 ]; then echo "警告：提取音频失败，文件: ${video_full_path}"; fi; \
done
```