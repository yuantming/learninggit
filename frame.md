# Frame Extraction Service Flow

## Overview
The Frame Extraction Service is responsible for extracting keyframes from processed videos for AI analysis. This service follows the video processing step and prepares individual frames for efficient AI processing.

## 方案说明 (Solution Description)

### 核心功能 (Core Functionality)
帧提取服务的核心功能是从已解码的视频中提取关键帧，为后续的AI分析做准备。该服务接收来自视频处理服务的已验证视频数据，专注于高效的帧提取过程。

### 技术选型 (Technology Selection)
- **帧提取库**: 使用JavaCV库进行视频帧提取，该库提供了对FFmpeg的Java封装，能够高效处理MP4视频格式
- **云存储**: 采用AWS S3作为关键帧存储，提供高可用性和可扩展性
- **容器化部署**: 服务部署在AWS EKS上，支持水平扩展以应对不同负载

### 提取策略 (Extraction Strategy)
- **基础策略**: 每秒提取1帧作为关键帧，平衡处理效率和分析准确性
- **增强策略**: 根据测试结果和检测准确率要求，可调整为每秒1-3-5帧的提取频率
- **格式支持**: 输出格式为JPG/PNG，确保与AI模型兼容

### 内存优化 (Memory Optimization)
- 采用流式处理，避免将所有帧同时加载到内存中
- 每帧独立处理和存储，降低内存占用
- 支持按需从S3检索帧进行处理

## 处理流程 (Processing Flow)

```mermaid
graph LR
    A[接收已解码视频] --> B[提取关键帧]
    B --> C[存储关键帧到S3]
    C --> D[通知AI编排服务]
```

## Detailed Steps

### 1. Input Video
- Receives decoded video from the Video Processing Service
- Video is in MP4 format, approximately 30 seconds long
- Video has already been validated and decoded

### 2. Extract Keyframes
- Extract keyframes at a base rate of 1 frame per second
- Enhanced strategy can extract 1-3-5 frames per second based on detection accuracy requirements
- Uses efficient extraction algorithms to minimize processing time
- Output format: JPG/PNG images

### 3. Store Keyframes in S3
- Each extracted keyframe is uploaded to dedicated S3 storage
- Keyframes are organized by video ID for easy retrieval
- Metadata about extracted frames is recorded

### 4. Notify AI Orchestration Service
- Send notification to AI Orchestration Service that keyframes are ready
- Include references to the stored keyframes
- Trigger the AI analysis workflow

## Technical Implementation Considerations

### Memory Efficiency
- Frames are processed and stored individually to avoid memory overload
- No need to keep all frames in memory simultaneously
- On-demand retrieval from S3 for processing

### Scalability
- Can process multiple videos in parallel
- Each video's frames are handled independently
- Horizontal scaling supported through container orchestration

### Error Handling
- Retry mechanism for failed frame extractions
- Dead letter queue for consistently failing videos
- Logging and monitoring for troubleshooting
