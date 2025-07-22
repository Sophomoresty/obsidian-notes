## yt-dlp 下载系列视频
```bash
mamba run -n 替换为你的虚拟环境名 yt-dlp \
    -U \
    --cookies "替换为你的B站cookie文件路径" \
    -f "bv*+ba/b" \
    -o "替换为你的下载文件放置的地址" \
    # 如果想多线程, 下载速度快点, 这里可以设置下载器为aria2c, 需要单独安装
    # --downloader "aria2c" \
    # --downloader-args "aria2c:-j5 -x16" \
    
    
    # 注意要获取最高画质和音质, 必须导入cookie+大会员;
    # cookie+普通会员, 只能下载普通用户下能观看的画质和音质
	# 下面可以放多个地址, 番剧, 播放列表, 具体视频, 电影都即可
    "https://space.bilibili.com/67815486/lists/290421" \

```


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
    "https://space.bilibili.com/67815486/lists/290421"

```



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
echo ""                              # 打印空行，确保"开始下载"前有间隔

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

## yt-dlp 下载音频

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

## ari2c下载示例
aria2c \
    --input-file="/mnt/d/Download/chrome download/桥桥超温柔.m3u8" \
    --dir="/mnt/c/Users/Sophomores/Desktop/桥桥超温柔/" \
    --max-concurrent-downloads=5 \
    --max-connection-per-server=16


## 下载视频和音频

```bash
#!/bin/bash

# --- 配置区 ---
COOKIES_FILE="/mnt/d/Download/chrome download/www.bilibili.com_cookies.txt" # cookie地址
VIDEO_ROOT_DIR="/mnt/c/Users/Sophomores/Desktop/7_16" # 下载的视频地址

# 要下载的B站URL列表
URLS=(
	"https://www.bilibili.com/video/BV1QM411d79A/"
	"https://www.bilibili.com/video/BV1Ws411Z7rt/"
	"https://www.bilibili.com/video/BV1dN4y1W7Hc/"
	"https://www.bilibili.com/video/BV1o8411u7Sn/"
	"https://www.bilibili.com/video/BV1E2421T739/"
	"https://www.bilibili.com/video/BV1Mp4y1v7sk/"
	"https://www.bilibili.com/video/BV1UDqZYyEsb/"
	"https://www.bilibili.com/video/BV1FV411h7BE/"
	
)

echo "--- 开始下载视频 ---"

YT_DLP_OUTPUT_TEMPLATE="${VIDEO_ROOT_DIR}/%(playlist)s/%(title)s.%(ext)s"

mamba run -n yt_dlp_enve yt-dlp \
  -U \
  --cookies "${COOKIES_FILE}" \
  --download-archive "bilibili_archive.txt" \
  -f "bv*+ba/b" \
  --output "${YT_DLP_OUTPUT_TEMPLATE}" \
  --downloader "aria2c" \
  --downloader-args "aria2c:-j5 -x16" \
  "${URLS[@]}"
echo "--- 下载结束 ---"
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
## yt-dlp 批量下载视频和音频, 填入封面和元信息

```bash
#!/bin/bash

# --- 配置区 ---
COOKIES_FILE="/mnt/d/Download/chrome download/www.bilibili.com_cookies.txt" # cookie地址
MAIN_BASE_DIR="/mnt/c/Users/Sophomores/Desktop/7_16_ultimate" # 主目录，所有输出将在这里
DOWNLOAD_ARCHIVE_FILE="${MAIN_BASE_DIR}/bilibili_archive.txt" # yt-dlp下载记录文件

VIDEO_DIR="${MAIN_BASE_DIR}/视频" # 视频存放目录
AUDIO_DIR="${MAIN_BASE_DIR}/音频" # 音频存放目录
COVER_DIR="${MAIN_BASE_DIR}/封面" # 封面存放目录
TEMP_PROCESSING_DIR="/tmp/media_processing_temp" # 临时文件存放目录 (会在脚本结束时清理)

# 要下载的B站URL列表
URLS=(
	"https://www.bilibili.com/video/BV1QM411d79A/"
	"https://www.bilibili.com/video/BV1Ws411Z7rt/"
	"https://www.bilibili.com/video/BV1dN4y1W7Hc/"
	"https://www.com/video/BV1o8411u7Sn/"
	"https://www.bilibili.com/video/BV1E2421T739/"
	"https://www.bilibili.com/video/BV1Mp4y1v7sk/"
	"https://www.bilibili.com/video/BV1UDqZYyEsb/"
	"https://www.bilibili.com/video/BV1FV411h7BE/"
	"https://space.bilibili.com/67815486/lists/290421" # 包含播放列表的URL作为示例
    "https://space.bilibili.com/6065166/lists/1129502" # 包含播放列表的URL作为示例
    "https://www.bilibili.com/video/BV1F64y1m74E/"
    "https://www.bilibili.com/video/BV13z4y1o78c"
    "https://www.bilibili.com/video/BV1aJ411S7sJ/"
    "https://www.bilibili.com/video/BV1sB4y1G7zm/"
    "https://www.bilibili.com/video/BV1i7411v7PF/"
    "https://www.bilibili.com/video/BV1bo4y1377c/"
    "https://www.bilibili.com/video/BV17b421J7bW/"
)

# --- 检查依赖 ---
if ! command -v mamba &> /dev/null; then
    echo "错误：mamba 未安装或不在 PATH 中。"
    exit 1
fi
if ! command -v ffmpeg &> /dev/null; then
    echo "错误：ffmpeg 未安装或不在 PATH 中。请安装 ffmpeg (sudo apt install ffmpeg)。"
    exit 1
fi
if ! command -v jq &> /dev/null; then
    echo "错误：jq 未安装或不在 PATH 中。请安装 jq (sudo apt install jq)。"
    exit 1
fi

# 确保所有目标目录存在
mkdir -p "${VIDEO_DIR}" "${AUDIO_DIR}" "${COVER_DIR}" "${TEMP_PROCESSING_DIR}"

# --- 阶段 1: 使用 yt-dlp 下载视频、封面和元数据 ---
echo "--- 阶段 1: 开始下载视频、封面和元数据 ---"
mamba run -n yt_dlp_enve yt-dlp \
  -U \
  --cookies "${COOKIES_FILE}" \
  --download-archive "${DOWNLOAD_ARCHIVE_FILE}" \
  -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best" \
  --output "${VIDEO_DIR}/%(playlist)s/%(title)s.%(ext)s" \
  --write-thumbnail \
  --embed-metadata \
  --write-info-json \
  --downloader "aria2c" \
  --downloader-args "aria2c:-j5 -x16" \
  "${URLS[@]}"

if [ $? -ne 0 ]; then
    echo "警告：yt-dlp 下载视频/封面/元数据时可能存在错误，但继续尝试处理文件。"
fi
echo "--- 阶段 1: 视频、封面和元数据下载完成 ---"

# 暂停一小会儿，确保文件系统操作完成
sleep 2

# --- 阶段 2: 移动封面到指定目录 ---
echo "--- 阶段 2: 移动封面到指定目录 ---"
# 启用 globstar 选项，允许递归匹配目录
shopt -s globstar

# 遍历视频目录及其所有子目录中的所有图片文件 (jpg, webp)
for cover_temp_path in "${VIDEO_DIR}"/**/*.{jpg,webp}; do
  if [ ! -f "${cover_temp_path}" ]; then
    continue
  fi

  # 获取相对于 VIDEO_DIR 的路径 (例如：播放列表名/封面标题.jpg)
  relative_path="${cover_temp_path#${VIDEO_DIR}/}"
  
  # 构建封面在 COVER_DIR 中的最终路径
  final_cover_path="${COVER_DIR}/${relative_path}"

  # 确保目标目录存在
  mkdir -p "$(dirname "${final_cover_path}")"

  echo "移动封面: ${cover_temp_path} -> ${final_cover_path}"
  mv "${cover_temp_path}" "${final_cover_path}"
  if [ $? -ne 0 ]; then
    echo "警告：移动封面失败: ${cover_temp_path}"
  fi
done
echo "--- 阶段 2: 封面移动完成 ---"


# --- 阶段 3: 处理视频和音频文件 (写入封面、提取音频并嵌入封面和元数据) ---
echo "--- 阶段 3: 开始处理视频和音频文件 ---"

# 遍历 VIDEO_DIR 及其所有子目录中的 mp4 文件
for video_full_path in "${VIDEO_DIR}"/**/*.mp4; do
  # 检查文件是否存在，防止匹配到空结果
  if [ ! -f "${video_full_path}" ]; then
    continue
  fi

  echo "---"
  echo "处理视频文件: ${video_full_path}"

  # 获取相对于 VIDEO_DIR 的路径 (例如：播放列表名/视频标题.mp4)
  relative_video_path="${video_full_path#${VIDEO_DIR}/}"
  # 移除文件扩展名，得到基本名称 (例如：播放列表名/视频标题)
  base_name_no_ext="${relative_video_path%.mp4}"
  
  # 查找对应的 .info.json 文件 (它在视频文件同目录)
  info_json_file="${VIDEO_DIR}/${base_name_no_ext}.info.json"
  
  # 初始化元数据变量
  VIDEO_TITLE=""
  VIDEO_ARTIST=""
  VIDEO_ALBUM=""
  VIDEO_DATE=""
  VIDEO_DESCRIPTION=""

  if [ -f "${info_json_file}" ]; then
      # 使用 jq 从 JSON 文件中提取元数据
      VIDEO_TITLE=$(jq -r '.title // empty' "${info_json_file}")
      VIDEO_ARTIST=$(jq -r '.uploader // empty' "${info_json_file}")
      VIDEO_ALBUM=$(jq -r '.playlist // .title // empty' "${info_json_file}") # 如果有播放列表则用播放列表名，否则用视频标题作专辑名
      VIDEO_DATE=$(jq -r '.upload_date // empty' "${info_json_file}" | sed 's/\(....\)\(..\)\(..\)/\1-\2-\3/') # 格式化日期为 YYYY-MM-DD
      VIDEO_DESCRIPTION=$(jq -r '.description // empty' "${info_json_file}")
  fi

  # 查找对应的封面图片 (封面已经在 COVER_DIR)
  COVER_IMAGE=""
  if [ -f "${COVER_DIR}/${base_name_no_ext}.jpg" ]; then
    COVER_IMAGE="${COVER_DIR}/${base_name_no_ext}.jpg"
  elif [ -f "${COVER_DIR}/${base_name_no_ext}.png" ]; then # 检查 .png 格式
    COVER_IMAGE="${COVER_DIR}/${base_name_no_ext}.png"
  elif [ -f "${COVER_DIR}/${base_name_no_ext}.webp" ]; then # 以防万一，也检查一下 .webp
    COVER_IMAGE="${COVER_DIR}/${base_name_no_ext}.webp"
  fi

  # --- 3.1: 给视频文件本身写入封面 ---
  echo "  - 步骤 3.1: 给视频文件写入封面..."
  if [ -n "${COVER_IMAGE}" ]; then
      # 构建临时输出视频文件的路径
      TEMP_OUTPUT_VIDEO_PATH="${TEMP_PROCESSING_DIR}/temp_video_${base_name_no_ext##*/}_$(date +%s%N).mp4" # 确保唯一性
      mkdir -p "$(dirname "${TEMP_OUTPUT_VIDEO_PATH}")" # 确保临时目录存在

      # 使用经过验证的视频封面嵌入命令，输出到临时文件
      eval "ffmpeg -i \"${video_full_path}\" -i \"${COVER_IMAGE}\" -map 0:v:0 -map 0:a -map 1:v -c:v copy -c:a copy -disposition:v:1 attached_pic -y -hide_banner \"${TEMP_OUTPUT_VIDEO_PATH}\""
      
      if [ $? -eq 0 ] && [ -f "${TEMP_OUTPUT_VIDEO_PATH}" ]; then
          echo "    - 成功写入封面到临时视频，替换原文件..."
          # 成功后，删除原始文件，并重命名临时文件为原始文件名
          rm "${video_full_path}"
          mv "${TEMP_OUTPUT_VIDEO_PATH}" "${video_full_path}"
      else
          echo "警告：给视频文件写入封面失败或临时文件未生成: ${video_full_path}。跳过视频替换。"
          # 如果失败，删除可能残留的临时文件
          [ -f "${TEMP_OUTPUT_VIDEO_PATH}" ] && rm "${TEMP_OUTPUT_VIDEO_PATH}"
      fi
  else
      echo "警告：未找到封面图，跳过给视频文件写入封面: ${video_full_path}"
  fi

  # --- 3.2: 提取音频并嵌入封面和元数据 ---
  output_audio_path="${AUDIO_DIR}/${base_name_no_ext}.m4a"
  mkdir -p "$(dirname "${output_audio_path}")" # 确保音频输出目录存在

  echo "  - 步骤 3.2: 提取音频并嵌入封面和元数据..."
  TEMP_AUDIO_FILE="${TEMP_PROCESSING_DIR}/temp_audio_${base_name_no_ext##*/}_$(date +%s%N).m4a" # 确保唯一性
  mkdir -p "$(dirname "${TEMP_AUDIO_FILE}")" # 确保临时音频目录存在

  echo "    - 提取纯音频到临时文件..."
  ffmpeg -i "${video_full_path}" -map 0:a -c:a copy -y -hide_banner "${TEMP_AUDIO_FILE}"
  if [ $? -ne 0 ]; then
      echo "警告：提取纯音频失败，文件: ${video_full_path}。跳过后续音频处理。"
      [ -f "${TEMP_AUDIO_FILE}" ] && rm "${TEMP_AUDIO_FILE}"
      continue # 跳到下一个视频文件
  fi

  echo "    - 嵌入封面和元数据到最终音频文件..."
  # 构建元数据参数，只添加非空的元数据
  FFMPEG_METADATA_ARGS=""
  if [ -n "${VIDEO_TITLE}" ]; then FFMPEG_METADATA_ARGS+=" -metadata title=\"${VIDEO_TITLE}\""; fi
  if [ -n "${VIDEO_ARTIST}" ]; then FFMPEG_METADATA_ARGS+=" -metadata artist=\"${VIDEO_ARTIST}\""; fi
  if [ -n "${VIDEO_ALBUM}" ]; then FFMPEG_METADATA_ARGS+=" -metadata album=\"${VIDEO_ALBUM}\""; fi
  if [ -n "${VIDEO_DATE}" ]; then FFMPEG_METADATA_ARGS+=" -metadata date=\"${VIDEO_DATE}\""; fi
  if [ -n "${VIDEO_DESCRIPTION}" ]; then FFMPEG_METADATA_ARGS+=" -metadata description=\"${VIDEO_DESCRIPTION}\""; fi

  # 执行 FFmpeg 命令
  if [ -n "${COVER_IMAGE}" ]; then
      # 有封面图，使用双输入
      eval "ffmpeg -i \"${TEMP_AUDIO_FILE}\" -i \"${COVER_IMAGE}\" -map 0:a -map 1:v -c:a copy -c:v copy -disposition:v:0 attached_pic ${FFMPEG_METADATA_ARGS} -y -hide_banner \"${output_audio_path}\""
  else
      # 没有封面图，只处理音频和元数据
      eval "ffmpeg -i \"${TEMP_AUDIO_FILE}\" -map 0:a -c:a copy ${FFMPEG_METADATA_ARGS} -y -hide_banner \"${output_audio_path}\""
  fi

  if [ $? -ne 0 ]; then
      echo "警告：嵌入封面/元数据到音频失败，文件: ${video_full_path}"
  fi

  # 清理临时音频文件
  rm "${TEMP_AUDIO_FILE}"

done

echo "--- 阶段 3: 视频和音频处理完成 ---"

# --- 阶段 4: 清理 .info.json 文件 ---
echo "--- 阶段 4: 清理 .info.json 文件 ---"
# 再次启用 globstar，确保能遍历所有子目录
shopt -s globstar

# 遍历视频目录及其所有子目录中的所有 .info.json 文件并删除
for json_file_path in "${VIDEO_DIR}"/**/*.info.json; do
  if [ -f "${json_file_path}" ]; then
    echo "删除 JSON 文件: ${json_file_path}"
    rm "${json_file_path}"
  fi
done
echo "--- 阶段 4: .info.json 文件清理完成 ---"

# 清理所有其他临时目录
echo "--- 清理所有其他临时文件和目录 ---"
rm -rf "${TEMP_PROCESSING_DIR}"
echo "--- 所有任务完成 ---"
```

```bash
#!/bin/bash

# --- 配置区 ---
COOKIES_FILE="/mnt/d/Download/chrome download/www.bilibili.com_cookies.txt" # cookie地址
MAIN_BASE_DIR="/mnt/c/Users/Sophomores/Desktop/7_16_ultimate" # 主目录，所有输出将在这里
DOWNLOAD_ARCHIVE_FILE="${MAIN_BASE_DIR}/bilibili_archive.txt" # yt-dlp下载记录文件

VIDEO_DIR="${MAIN_BASE_DIR}/视频" # 视频存放目录
AUDIO_DIR="${MAIN_BASE_DIR}/音频" # 音频存放目录
COVER_DIR="${MAIN_BASE_DIR}/封面" # 封面存放目录
TEMP_PROCESSING_DIR="/tmp/media_processing_temp" # 临时文件存放目录 (会在脚本结束时清理)

# 要下载的B站URL列表
URLS=(
	"https://www.bilibili.com/video/BV1QM411d79A/"
	"https://www.bilibili.com/video/BV1Ws411Z7rt/"
	"https://www.bilibili.com/video/BV1dN4y1W7Hc/"
	"https://www.bilibili.com/video/BV1o8411u7Sn/"
	"https://www.bilibili.com/video/BV1E2421T739/"
	"https://www.bilibili.com/video/BV1Mp4y1v7sk/"
	"https://www.bilibili.com/video/BV1UDqZYyEsb/"
	"https://www.bilibili.com/video/BV1FV411h7BE/"
	"https://space.bilibili.com/67815486/lists/290421" # 包含播放列表的URL作为示例
    "https://space.bilibili.com/6065166/lists/1129502" # 包含播放列表的URL作为示例
    "https://www.bilibili.com/video/BV1F64y1m74E/"
    "https://www.bilibili.com/video/BV13z4y1o78c"
    "https://www.bilibili.com/video/BV1aJ411S7sJ/"
    "https://www.bilibili.com/video/BV1sB4y1G7zm/"
    "https://www.bilibili.com/video/BV1i7411v7PF/"
    "https://www.bilibili.com/video/BV1bo4y1377c/"
    "https://www.bilibili.com/video/BV17b421J7bW/"
)

# --- 检查依赖 ---
if ! command -v mamba &> /dev/null; then
    echo "错误：mamba 未安装或不在 PATH 中。"
    exit 1
fi
if ! command -v ffmpeg &> /dev/null; then
    echo "错误：ffmpeg 未安装或不在 PATH 中。请安装 ffmpeg (sudo apt install ffmpeg)。"
    exit 1
fi
if ! command -v jq &> /dev/null; then
    echo "错误：jq 未安装或不在 PATH 中。请安装 jq (sudo apt install jq)。"
    exit 1
fi

# 确保所有目标目录存在
mkdir -p "${VIDEO_DIR}" "${AUDIO_DIR}" "${COVER_DIR}" "${TEMP_PROCESSING_DIR}"

# --- 阶段 1: 使用 yt-dlp 下载视频、封面和元数据 ---
echo "--- 阶段 1: 开始下载视频、封面和元数据 ---"
mamba run -n yt_dlp_enve yt-dlp \
  -U \
  --cookies "${COOKIES_FILE}" \
  --download-archive "${DOWNLOAD_ARCHIVE_FILE}" \
  -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best" \
  --output "${VIDEO_DIR}/%(playlist)s/%(title)s.%(ext)s" \
  --write-thumbnail \
  --embed-metadata \
  --write-info-json \
  --downloader "aria2c" \
  --downloader-args "aria2c:-j5 -x16" \
  "${URLS[@]}"

if [ $? -ne 0 ]; then
    echo "警告：yt-dlp 下载视频/封面/元数据时可能存在错误，但继续尝试处理文件。"
fi
echo "--- 阶段 1: 视频、封面和元数据下载完成 ---"

# 暂停一小会儿，确保文件系统操作完成
sleep 2

# --- 阶段 2: 移动封面到指定目录 ---
echo "--- 阶段 2: 移动封面到指定目录 ---"
# 启用 globstar 选项，允许递归匹配目录
shopt -s globstar

# 遍历视频目录及其所有子目录中的所有图片文件 (jpg, webp)
for cover_temp_path in "${VIDEO_DIR}"/**/*.{jpg,webp}; do
  if [ ! -f "${cover_temp_path}" ]; then
    continue
  fi

  # 获取相对于 VIDEO_DIR 的路径 (例如：播放列表名/封面标题.jpg)
  relative_path="${cover_temp_path#${VIDEO_DIR}/}"
  
  # 构建封面在 COVER_DIR 中的最终路径
  final_cover_path="${COVER_DIR}/${relative_path}"

  # 确保目标目录存在
  mkdir -p "$(dirname "${final_cover_path}")"

  echo "移动封面: ${cover_temp_path} -> ${final_cover_path}"
  mv "${cover_temp_path}" "${final_cover_path}"
  if [ $? -ne 0 ]; then
    echo "警告：移动封面失败: ${cover_temp_path}"
  fi
done
echo "--- 阶段 2: 封面移动完成 ---"


# --- 阶段 3: 处理视频和音频文件 (写入封面、提取音频并嵌入封面和元数据) ---
echo "--- 阶段 3: 开始处理视频和音频文件 ---"

# 遍历 VIDEO_DIR 及其所有子目录中的 mp4 文件
for video_full_path in "${VIDEO_DIR}"/**/*.mp4; do
  # 检查文件是否存在，防止匹配到空结果
  if [ ! -f "${video_full_path}" ]; then
    continue
  fi

  echo "---"
  echo "处理视频文件: ${video_full_path}"

  # 获取相对于 VIDEO_DIR 的路径 (例如：播放列表名/视频标题.mp4)
  relative_video_path="${video_full_path#${VIDEO_DIR}/}"
  # 移除文件扩展名，得到基本名称 (例如：播放列表名/视频标题)
  base_name_no_ext="${relative_video_path%.mp4}"
  
  # 查找对应的 .info.json 文件 (它在视频文件同目录)
  info_json_file="${VIDEO_DIR}/${base_name_no_ext}.info.json"
  
  # 初始化元数据变量
  VIDEO_TITLE=""
  VIDEO_ARTIST=""
  VIDEO_ALBUM=""
  VIDEO_DATE=""
  VIDEO_DESCRIPTION=""

  if [ -f "${info_json_file}" ]; then
      # 使用 jq 从 JSON 文件中提取元数据
      VIDEO_TITLE=$(jq -r '.title // empty' "${info_json_file}")
      VIDEO_ARTIST=$(jq -r '.uploader // empty' "${info_json_file}")
      VIDEO_ALBUM=$(jq -r '.playlist // .title // empty' "${info_json_file}") # 如果有播放列表则用播放列表名，否则用视频标题作专辑名
      VIDEO_DATE=$(jq -r '.upload_date // empty' "${info_json_file}" | sed 's/\(....\)\(..\)\(..\)/\1-\2-\3/') # 格式化日期为 YYYY-MM-DD
      VIDEO_DESCRIPTION=$(jq -r '.description // empty' "${info_json_file}")
  fi

  # 查找对应的封面图片 (封面已经在 COVER_DIR)
  COVER_IMAGE=""
  if [ -f "${COVER_DIR}/${base_name_no_ext}.jpg" ]; then
    COVER_IMAGE="${COVER_DIR}/${base_name_no_ext}.jpg"
  elif [ -f "${COVER_DIR}/${base_name_no_ext}.png" ]; then # 检查 .png 格式
    COVER_IMAGE="${COVER_DIR}/${base_name_no_ext}.png"
  elif [ -f "${COVER_DIR}/${base_name_no_ext}.webp" ]; then # 以防万一，也检查一下 .webp
    COVER_IMAGE="${COVER_DIR}/${base_name_no_ext}.webp"
  fi

  # --- 3.1: 给视频文件本身写入封面 ---
  echo "  - 步骤 3.1: 给视频文件写入封面..."
  if [ -n "${COVER_IMAGE}" ]; then
      # 构建临时输出视频文件的路径
      TEMP_OUTPUT_VIDEO_PATH="${TEMP_PROCESSING_DIR}/temp_video_${base_name_no_ext##*/}_$(date +%s%N).mp4" # 确保唯一性
      mkdir -p "$(dirname "${TEMP_OUTPUT_VIDEO_PATH}")" # 确保临时目录存在

      # 使用经过验证的视频封面嵌入命令，输出到临时文件
      eval "ffmpeg -i \"${video_full_path}\" -i \"${COVER_IMAGE}\" -map 0:v:0 -map 0:a -map 1:v -c:v copy -c:a copy -disposition:v:1 attached_pic -y -hide_banner \"${TEMP_OUTPUT_VIDEO_PATH}\""
      
      if [ $? -eq 0 ] && [ -f "${TEMP_OUTPUT_VIDEO_PATH}" ]; then
          echo "    - 成功写入封面到临时视频，替换原文件..."
          # 成功后，删除原始文件，并重命名临时文件为原始文件名
          rm "${video_full_path}"
          mv "${TEMP_OUTPUT_VIDEO_PATH}" "${video_full_path}"
      else
          echo "警告：给视频文件写入封面失败或临时文件未生成: ${video_full_path}。跳过视频替换。"
          # 如果失败，删除可能残留的临时文件
          [ -f "${TEMP_OUTPUT_VIDEO_PATH}" ] && rm "${TEMP_OUTPUT_VIDEO_PATH}"
      fi
  else
      echo "警告：未找到封面图，跳过给视频文件写入封面: ${video_full_path}"
  fi

  # --- 3.2: 提取音频并嵌入封面和元数据 ---
  output_audio_path="${AUDIO_DIR}/${base_name_no_ext}.m4a"
  mkdir -p "$(dirname "${output_audio_path}")" # 确保音频输出目录存在

  echo "  - 步骤 3.2: 提取音频并嵌入封面和元数据..."
  TEMP_AUDIO_FILE="${TEMP_PROCESSING_DIR}/temp_audio_${base_name_no_ext##*/}_$(date +%s%N).m4a" # 确保唯一性
  mkdir -p "$(dirname "${TEMP_AUDIO_FILE}")" # 确保临时音频目录存在

  echo "    - 提取纯音频到临时文件..."
  ffmpeg -i "${video_full_path}" -map 0:a -c:a copy -y -hide_banner "${TEMP_AUDIO_FILE}"
  if [ $? -ne 0 ]; then
      echo "警告：提取纯音频失败，文件: ${video_full_path}。跳过后续音频处理。"
      [ -f "${TEMP_AUDIO_FILE}" ] && rm "${TEMP_AUDIO_FILE}"
      continue # 跳到下一个视频文件
  fi

  echo "    - 嵌入封面和元数据到最终音频文件..."
  # 构建元数据参数，只添加非空的元数据
  FFMPEG_METADATA_ARGS=""
  if [ -n "${VIDEO_TITLE}" ]; then FFMPEG_METADATA_ARGS+=" -metadata title=\"${VIDEO_TITLE}\""; fi
  if [ -n "${VIDEO_ARTIST}" ]; then FFMPEG_METADATA_ARGS+=" -metadata artist=\"${VIDEO_ARTIST}\""; fi
  if [ -n "${VIDEO_ALBUM}" ]; then FFMPEG_METADATA_ARGS+=" -metadata album=\"${VIDEO_ALBUM}\""; fi
  if [ -n "${VIDEO_DATE}" ]; then FFMPEG_METADATA_ARGS+=" -metadata date=\"${VIDEO_DATE}\""; fi
  if [ -n "${VIDEO_DESCRIPTION}" ]; then FFMPEG_METADATA_ARGS+=" -metadata description=\"${VIDEO_DESCRIPTION}\""; fi

  # 执行 FFmpeg 命令
  if [ -n "${COVER_IMAGE}" ]; then
      # 有封面图，使用双输入
      eval "ffmpeg -i \"${TEMP_AUDIO_FILE}\" -i \"${COVER_IMAGE}\" -map 0:a -map 1:v -c:a copy -c:v copy -disposition:v:0 attached_pic ${FFMPEG_METADATA_ARGS} -y -hide_banner \"${output_audio_path}\""
  else
      # 没有封面图，只处理音频和元数据
      eval "ffmpeg -i \"${TEMP_AUDIO_FILE}\" -map 0:a -c:a copy ${FFMPEG_METADATA_ARGS} -y -hide_banner \"${output_audio_path}\""
  fi

  if [ $? -ne 0 ]; then
      echo "警告：嵌入封面/元数据到音频失败，文件: ${video_full_path}"
  fi

  # 清理临时音频文件
  rm "${TEMP_AUDIO_FILE}"

done

echo "--- 阶段 3: 视频和音频处理完成 ---"

# --- 阶段 4: 清理 .info.json 文件 ---
echo "--- 阶段 4: 清理 .info.json 文件 ---"
# 再次启用 globstar，确保能遍历所有子目录
shopt -s globstar

# 遍历视频目录及其所有子目录中的所有 .info.json 文件并删除
for json_file_path in "${VIDEO_DIR}"/**/*.info.json; do
  if [ -f "${json_file_path}" ]; then
    echo "删除 JSON 文件: ${json_file_path}"
    rm "${json_file_path}"
  fi
done
echo "--- 阶段 4: .info.json 文件清理完成 ---"

# 清理所有其他临时目录
echo "--- 清理所有其他临时文件和目录 ---"
rm -rf "${TEMP_PROCESSING_DIR}"
echo "--- 所有任务完成 ---"
```
### 完善版本

```bash
#!/bin/bash

# --- 配置区 ---
COOKIES_FILE="/mnt/d/Download/chrome download/www.bilibili.com_cookies.txt" # cookie地址
MAIN_BASE_DIR="/mnt/c/Users/Sophomores/Desktop/7_16_ultimate_flex" # 主目录，所有输出将在这里
DOWNLOAD_ARCHIVE_FILE="${MAIN_BASE_DIR}/bilibili_archive.txt" # yt-dlp下载记录文件

VIDEO_DIR="${MAIN_BASE_DIR}/视频" # 视频存放目录
AUDIO_DIR="${MAIN_BASE_DIR}/音频" # 音频存放目录
COVER_DIR="${MAIN_BASE_DIR}/封面" # 封面存放目录
TEMP_PROCESSING_DIR="/tmp/media_processing_temp" # 临时文件存放目录 (会在脚本结束时清理)

# --- 是否处理音频 (true/false) ---
PROCESS_AUDIO="true" # 设置为 "true" 会提取音频并嵌入封面/元数据；设置为 "false" 则跳过此阶段

# 要下载的B站URL列表
URLS=(
	"https://www.bilibili.com/video/BV1QM411d79A/"
	"https://www.bilibili.com/video/BV1Ws411Z7rt/"
	"https://www.bilibili.com/video/BV1dN4y1W7Hc/"
	"https://www.bilibili.com/video/BV1o8411u7Sn/"
	"https://www.bilibili.com/video/BV1E2421T739/"
	"https://www.bilibili.com/video/BV1Mp4y1v7sk/"
	"https://www.bilibili.com/video/BV1UDqZYyEsb/"
	"https://www.bilibili.com/video/BV1FV411h7BE/"
	"https://space.bilibili.com/67815486/lists/290421" # 包含播放列表的URL作为示例
    "https://space.bilibili.com/6065166/lists/1129502" # 包含播放列表的URL作为示例
    "https://www.bilibili.com/video/BV1F64y1m74E/"
    "https://www.bilibili.com/video/BV13z4y1o78c"
    "https://www.bilibili.com/video/BV1aJ411S7sJ/"
    "https://www.bilibili.com/video/BV1sB4y1G7zm/"
    "https://www.bilibili.com/video/BV1i7411v7PF/"
    "https://www.bilibili.com/video/BV1bo4y1377c/"
    "https://www.bilibili.com/video/BV17b421J7bW/"
)

# --- 检查依赖 ---
if ! command -v mamba &> /dev/null; then
    echo "错误：mamba 未安装或不在 PATH 中。"
    exit 1
fi
if ! command -v ffmpeg &> /dev/null; then
    echo "错误：ffmpeg 未安装或不在 PATH 中。请安装 ffmpeg (sudo apt install ffmpeg)。"
    exit 1
fi
if ! command -v jq &> /dev/null; then
    echo "错误：jq 未安装或不在 PATH 中。请安装 jq (sudo apt install jq)。"
    exit 1
fi

# 确保必要的目录存在 (排除 AUDIO_DIR)
mkdir -p "${VIDEO_DIR}" "${COVER_DIR}" "${TEMP_PROCESSING_DIR}" # <<<--- AUDIO_DIR 已移除

# --- 阶段 1: 使用 yt-dlp 下载视频、封面和元数据 ---
echo "--- 阶段 1: 开始下载视频、封面和元数据 ---"
mamba run -n yt_dlp_enve yt-dlp \
  -U \
  --cookies "${COOKIES_FILE}" \
  --download-archive "${DOWNLOAD_ARCHIVE_FILE}" \
  -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best" \
  --output "${VIDEO_DIR}/%(playlist)s/%(title)s.%(ext)s" \
  --write-thumbnail \
  --embed-metadata \
  --write-info-json \
  --downloader "aria2c" \
  --downloader-args "aria2c:-j5 -x16" \
  "${URLS[@]}"

if [ $? -ne 0 ]; then
    echo "警告：yt-dlp 下载视频/封面/元数据时可能存在错误，但继续尝试处理文件。"
fi
echo "--- 阶段 1: 视频、封面和元数据下载完成 ---"

# 暂停一小会儿，确保文件系统操作完成
sleep 2

# --- 阶段 2: 移动封面到指定目录 ---
echo "--- 阶段 2: 移动封面到指定目录 ---"
# 启用 globstar 选项，允许递归匹配目录
shopt -s globstar

# 遍历视频目录及其所有子目录中的所有图片文件 (jpg, webp)
for cover_temp_path in "${VIDEO_DIR}"/**/*.{jpg,webp}; do
  if [ ! -f "${cover_temp_path}" ]; then
    continue
  fi

  # 获取相对于 VIDEO_DIR 的路径 (例如：播放列表名/封面标题.jpg)
  relative_path="${cover_temp_path#${VIDEO_DIR}/}"
  
  # 构建封面在 COVER_DIR 中的最终路径
  final_cover_path="${COVER_DIR}/${relative_path}"

  # 确保目标目录存在
  mkdir -p "$(dirname "${final_cover_path}")"

  echo "移动封面: ${cover_temp_path} -> ${final_cover_path}"
  mv "${cover_temp_path}" "${final_cover_path}"
  if [ $? -ne 0 ]; then
    echo "警告：移动封面失败: ${cover_temp_path}"
  fi
done
echo "--- 阶段 2: 封面移动完成 ---"


# --- 阶段 3: 处理视频和音频文件 (写入封面、提取音频并嵌入封面和元数据) ---
echo "--- 阶段 3: 开始处理视频和音频文件 ---"

# 遍历 VIDEO_DIR 及其所有子目录中的 mp4 文件
for video_full_path in "${VIDEO_DIR}"/**/*.mp4; do
  # 检查文件是否存在，防止匹配到空结果
  if [ ! -f "${video_full_path}" ]; then
    continue
  fi

  echo "---"
  echo "处理视频文件: ${video_full_path}"

  # 获取相对于 VIDEO_DIR 的路径 (例如：播放列表名/视频标题.mp4)
  relative_video_path="${video_full_path#${VIDEO_DIR}/}"
  # 移除文件扩展名，得到基本名称 (例如：播放列表名/视频标题)
  base_name_no_ext="${relative_video_path%.mp4}" # ！！！这个就是文件名（不含扩展名）

  # 查找对应的 .info.json 文件 (它在视频文件同目录)
  info_json_file="${VIDEO_DIR}/${base_name_no_ext}.info.json"
  
  # 初始化元数据变量
  # VIDEO_TITLE 变更为直接使用文件名
  VIDEO_TITLE="${base_name_no_ext}" # <<<--- 这里使用文件名作为标题

  VIDEO_ARTIST=""
  VIDEO_ALBUM=""
  VIDEO_DATE=""
  VIDEO_DESCRIPTION=""

  if [ -f "${info_json_file}" ]; then
      # 使用 jq 从 JSON 文件中提取元数据，但不提取title
      VIDEO_ARTIST=$(jq -r '.uploader // empty' "${info_json_file}")
      VIDEO_ALBUM=$(jq -r '.playlist // .title // empty' "${info_json_file}") # 如果有播放列表则用播放列表名，否则用视频标题作专辑名
      VIDEO_DATE=$(jq -r '.upload_date // empty' "${info_json_file}" | sed 's/\(....\)\(..\)\(..\)/\1-\2-\3/') # 格式化日期为 YYYY-MM-DD
      VIDEO_DESCRIPTION=$(jq -r '.description // empty' "${info_json_file}")
  fi

  # 查找对应的封面图片 (封面已经在 COVER_DIR)
  COVER_IMAGE=""
  if [ -f "${COVER_DIR}/${base_name_no_ext}.jpg" ]; then
    COVER_IMAGE="${COVER_DIR}/${base_name_no_ext}.jpg"
  elif [ -f "${COVER_DIR}/${base_name_no_ext}.png" ]; then # 检查 .png 格式
    COVER_IMAGE="${COVER_DIR}/${base_name_no_ext}.png"
  elif [ -f "${COVER_DIR}/${base_name_no_ext}.webp" ]; then # 以防万一，也检查一下 .webp
    COVER_IMAGE="${COVER_DIR}/${base_name_no_ext}.webp"
  fi

  # --- 3.1: 给视频文件本身写入封面 ---
  echo "  - 步骤 3.1: 给视频文件写入封面..."
  if [ -n "${COVER_IMAGE}" ]; then
      # 构建临时输出视频文件的路径
      TEMP_OUTPUT_VIDEO_PATH="${TEMP_PROCESSING_DIR}/temp_video_${base_name_no_ext##*/}_$(date +%s%N).mp4" # 确保唯一性
      mkdir -p "$(dirname "${TEMP_OUTPUT_VIDEO_PATH}")" # 确保临时目录存在

      # 使用经过验证的视频封面嵌入命令，输出到临时文件
      # 注意：给视频嵌入封面时，这里的title也是由yt-dlp embed-metadata写入的，与文件名一致
      eval "ffmpeg -i \"${video_full_path}\" -i \"${COVER_IMAGE}\" -map 0:v:0 -map 0:a -map 1:v -c:v copy -c:a copy -disposition:v:1 attached_pic -y -hide_banner \"${TEMP_OUTPUT_VIDEO_PATH}\""
      
      if [ $? -eq 0 ] && [ -f "${TEMP_OUTPUT_VIDEO_PATH}" ]; then
          echo "    - 成功写入封面到临时视频，替换原文件..."
          # 成功后，删除原始文件，并重命名临时文件为原始文件名
          rm "${video_full_path}"
          mv "${TEMP_OUTPUT_VIDEO_PATH}" "${video_full_path}"
      else
          echo "警告：给视频文件写入封面失败或临时文件未生成: ${video_full_path}。跳过视频替换。"
          # 如果失败，删除可能残留的临时文件
          [ -f "${TEMP_OUTPUT_VIDEO_PATH}" ] && rm "${TEMP_OUTPUT_VIDEO_PATH}"
      fi
  else
      echo "警告：未找到封面图，跳过给视频文件写入封面: ${video_full_path}"
  fi

  # --- 3.2: 提取音频并嵌入封面和元数据 ---
  if [ "${PROCESS_AUDIO}" = "true" ]; then # <<<--- 新增的判断！
      # 仅当 PROCESS_AUDIO 为 true 时才创建 AUDIO_DIR 的子目录
      # 在此之前，确保 AUDIO_DIR 根目录已创建 (通过在 if 块开始处创建)
      mkdir -p "${AUDIO_DIR}/${base_name_no_ext%/*}" # <<<--- 确保创建音频子目录 (仅在需要时)

      output_audio_path="${AUDIO_DIR}/${base_name_no_ext}.m4a"
      # mkdir -p "$(dirname "${output_audio_path}")" # 这行可以在上面那行之后移除，因为它会创建更深层

      echo "  - 步骤 3.2: 提取音频并嵌入封面和元数据..."
      TEMP_AUDIO_FILE="${TEMP_PROCESSING_DIR}/temp_audio_${base_name_no_ext##*/}_$(date +%s%N).m4a" # 确保唯一性
      mkdir -p "$(dirname "${TEMP_AUDIO_FILE}")" # 确保临时音频目录存在

      echo "    - 提取纯音频到临时文件..."
      ffmpeg -i "${video_full_path}" -map 0:a -c:a copy -y -hide_banner "${TEMP_AUDIO_FILE}"
      if [ $? -ne 0 ]; then
          echo "警告：提取纯音频失败，文件: ${video_full_path}。跳过后续音频处理。"
          [ -f "${TEMP_AUDIO_FILE}" ] && rm "${TEMP_AUDIO_FILE}"
          continue # 跳到下一个视频文件
      fi

      echo "    - 嵌入封面和元数据到最终音频文件..."
      # 构建元数据参数，只添加非空的元数据
      FFMPEG_METADATA_ARGS=""
      if [ -n "${VIDEO_TITLE}" ]; then FFMPEG_METADATA_ARGS+=" -metadata title=\"${VIDEO_TITLE}\""; fi
      if [ -n "${VIDEO_ARTIST}" ]; then FFMPEG_METADATA_ARGS+=" -metadata artist=\"${VIDEO_ARTIST}\""; fi
      if [ -n "${VIDEO_ALBUM}" ]; then FFMPEG_METADATA_ARGS+=" -metadata album=\"${VIDEO_ALBUM}\""; fi
      if [ -n "${VIDEO_DATE}" ]; then FFMPEG_METADATA_ARGS+=" -metadata date=\"${VIDEO_DATE}\""; fi
      if [ -n "${VIDEO_DESCRIPTION}" ]; then FFMPEG_METADATA_ARGS+=" -metadata description=\"${VIDEO_DESCRIPTION}\""; fi

      # 执行 FFmpeg 命令
      if [ -n "${COVER_IMAGE}" ]; then
          # 有封面图，使用双输入
          eval "ffmpeg -i \"${TEMP_AUDIO_FILE}\" -i \"${COVER_IMAGE}\" -map 0:a -map 1:v -c:a copy -c:v copy -disposition:v:0 attached_pic ${FFMPEG_METADATA_ARGS} -y -hide_banner \"${output_audio_path}\""
      else
          # 没有封面图，只处理音频和元数据
          eval "ffmpeg -i \"${TEMP_AUDIO_FILE}\" -map 0:a -c:a copy ${FFMPEG_METADATA_ARGS} -y -hide_banner \"${output_audio_path}\""
      fi

      if [ $? -ne 0 ]; then
          echo "警告：嵌入封面/元数据到音频失败，文件: ${video_full_path}"
      fi

      # 清理临时音频文件
      rm "${TEMP_AUDIO_FILE}"
  else # PROCESS_AUDIO = false
      echo "  - 步骤 3.2: 跳过音频处理 (PROCESS_AUDIO 设置为 false)。"
  fi # <<<--- 判断结束

done

echo "--- 阶段 3: 视频和音频处理完成 ---"

# --- 阶段 4: 清理 .info.json 文件 ---
echo "--- 阶段 4: 清理 .info.json 文件 ---"
# 再次启用 globstar，确保能遍历所有子目录
shopt -s globstar

# 遍历视频目录及其所有子目录中的所有 .info.json 文件并删除
for json_file_path in "${VIDEO_DIR}"/**/*.info.json; do
  if [ -f "${json_file_path}" ]; then
    echo "删除 JSON 文件: ${json_file_path}"
    rm "${json_file_path}"
  fi
done
echo "--- 阶段 4: .info.json 文件清理完成 ---"

# 清理所有其他临时目录
echo "--- 清理所有其他临时文件和目录 ---"
rm -rf "${TEMP_PROCESSING_DIR}"
echo "--- 所有任务完成 ---"
```

## 已下载视频

### 此前下载过的部分
"https://space.bilibili.com/67815486/lists/290421"
"https://space.bilibili.com/6065166/lists/1129502"
"https://www.bilibili.com/video/BV1F64y1m74E/"
"https://www.bilibili.com/video/BV13z4y1o78c"
"https://www.bilibili.com/video/BV1aJ411S7sJ/"
"https://www.bilibili.com/video/BV1sB4y1G7zm/"
"https://www.bilibili.com/video/BV1i7411v7PF/"
"https://www.bilibili.com/video/BV1bo4y1377c/"
"https://www.bilibili.com/video/BV17b421J7bW/"
### 2025-07-16
- https://www.bilibili.com/video/BV1QM411d79A/
	- [45年后的大雄再遇已逝父母，早已物是人非](https://www.bilibili.com/video/BV1QM411d79A/)
- https://www.bilibili.com/video/BV1Ws411Z7rt/
	- [【多素材】喜欢你真的好痛苦](https://www.bilibili.com/video/BV1Ws411Z7rt/)
- https://www.bilibili.com/video/BV1dN4y1W7Hc/
	- 【原创动画PV】神之天平 重置版三渲二OP 一人制作三维动画绑定渲染后期
- https://www.bilibili.com/video/BV1o8411u7Sn/
	- "我愿意为你在潮湿中腐烂"
- https://www.bilibili.com/video/BV1E2421T739/
	- "付之一炬吧，我贱烂的生命"
- https://www.bilibili.com/video/BV1Mp4y1v7sk/
	- 「助眠神作」油管已销户作者助眠神作 手势催眠开山鼻祖 无人声
- https://www.bilibili.com/video/BV1UDqZYyEsb/
	- 写给自卑的自己的一封信
- https://www.bilibili.com/video/BV1FV411h7BE/
	- 质量超级超级高的隐形触发音，舒适度拉满真的爱了




