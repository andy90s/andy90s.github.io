# iOS录音功能

<!--more-->
## 前言
iOS原生API录音功能
## 代码
```swift
import AVFoundation
class Recorder {
    var recorder: AVAudioRecorder?
    func startRecord() {
        let session = AVAudioSession.sharedInstance()
        do {
            try session.setCategory(.playAndRecord, mode: .default, options: [])
            try session.setActive(true, options: [])
            session.requestRecordPermission { [unowned self] allowed in
                DispatchQueue.main.async {
                    if allowed {
                        self.startRecording()
                    } else {
                        // failed to record!
                    }
                }
            }
        } catch {
            // failed to record!
        }
    }
  
    func startRecording() {
        // 输出mav格式
        let audioFilename = getDocumentsDirectory().appendingPathComponent("recording.wav")
        let settings = [
            AVFormatIDKey: Int(kAudioFormatLinearPCM),
            AVSampleRateKey: 44100,
            AVNumberOfChannelsKey: 2,
            AVEncoderAudioQualityKey: AVAudioQuality.high.rawValue
        ]
        do {
            recorder = try AVAudioRecorder(url: audioFilename, settings: settings)
            recorder?.record()
        } catch {
            finishRecording(success: false)
        }
    }

    func finishRecording(success: Bool) {
        recorder?.stop()
        recorder = nil
        if success {
            print("Finished recording")
        } else {
            print("Failed recording")
        }
    }

    func getDocumentsDirectory() -> URL {
        let paths = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)
        return paths[0]
    }

    func stopRecord() {
        finishRecording(success: true)
    }
}
```
## 使用
```swift
let recorder = Recorder() // 初始化
recorder.startRecord() // 开始录音
recorder.stopRecord() // 结束录音
```

## 使用form-data上传
```swift
private func recordComplete() {
    // 1. 获取录音文件
    let filePath = recorder.getDocumentsDirectory().appendingPathComponent("recording.wav")
    // 2. 上传录音文件地址
    let uploadUrl = "http://xxx.com/upload"
    // 3. 设置请求头
    let headers: HTTPHeaders = [
        "Content-type": "multipart/form-data",
        "Accept": "application/json",
    ]
    // 4. 使用alamofire上传 
    AF.upload(multipartFormData: { (multipartFormData) in
        multipartFormData.append(filePath, withName: "file", fileName: "recoding.wav", mimeType: "audio/wav")
    }, to: uploadUrl, method: .post, headers: headers).responseJSON { (response) in
        switch response.result {
        case .success(let value):
            print(value)
        case .failure(let error):
            print("error: \(error)")
        }
    }
}
```

