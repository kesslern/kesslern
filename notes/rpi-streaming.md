# Raspberry Pi Camera and MotionEye

## Notes

- MotionEye on a Pi Zero is too slow to reasonably be used for much of anything.
- Use a basic generic OS to stream video via RTSP with [v4l2rtspserver](https://github.com/mpromonet/v4l2rtspserver).
- A more powerful computer running MotionEye can use this RTSP stream to do motion detection and save video locally or to a network device.

## Setup

First, enable the camera interface in `raspi-config` Go to `Interfacing Options` => `P1 Camera` => `Enable` and then reboot.

```shell
sudo apt-get install liblivemedia-dev liblog4cpp5-dev cmake
git clone https://github.com/mpromonet/v4l2rtspserver.git
cd v4l2rtspserver/
cmake .
make
sudo make install
```

## Command Reference
```shell
v4l2rtspserver -H 800 -W 600 -F 30 -P 8555 /dev/video0
```

## MotionEye Setup

Setup:
- Camera Device: `rtsp://[hostname]:8555/unicast`
- Camera Type: `Network Camera`

Movies:
- Format: MPEG4

### Systemd Service

1. Edit `/lib/systemd/system/v4l2rtspserver.service` and replace any existing parameters with the desired ones
2. Run `sudo systemctl enable v4l2rtspserver`
