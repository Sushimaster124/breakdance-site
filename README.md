# Breaking Pose Analyzer

Computer-vision analysis of competitive breaking (breakdance) battles. It takes a YouTube battle video,
tracks both dancers, extracts 17-point body pose for every frame and compares their rounds head-to-head.

**Live demo: https://sushimaster124.github.io/breakdance-site/**

This repo is the static site. The pose pipeline runs locally on Apple Silicon.

## Pipeline

1. **Download and clip** battle footage (up to 1080p) with yt-dlp and ffmpeg.
2. **Detect** every person in each frame with RTMDet.
3. **Track** the two dancers and split the video into each dancer's turns.
4. **Estimate pose** with RTMPose-L (17 COCO keypoints) on Apple MPS at ~34 fps.
   On the reference clip, 0% of frames had no detection and 8.7% had low confidence.
5. **Grade (experimental).** Transparent measurements from pose and audio, not a trained model:
   level changes, holds and freezes, and how well movement accents line up with the beat.
   Each dancer gets a share of the pair (50 = even), compared against the judges' real votes.

## Site

- **Analyzer** (`viewer/home.html`): when run locally, pick from 564 catalogued battles or paste a YouTube
  link to queue a new analysis. The live demo is a read-only snapshot of two analyzed battles.
- **Pose viewer** (`viewer/index.html`): video with a live skeleton overlay and a per-frame confidence timeline.

## Stack

Python, MMPose (RTMDet + RTMPose), PyTorch (MPS), NumPy, SciPy (Hungarian matching for tracking), yt-dlp, ffmpeg, vanilla JS.

## Limits

Pose captures variety, control and musicality. It can't capture creativity, personality or
performance, the rest of the WDSF judging criteria, so the grade is an aid to judging, not a judge.
