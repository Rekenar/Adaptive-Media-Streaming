# First try Settings (Big Buck Bunny 1080p 30fps)
ffmpeg 
-i input.mp4            
-c:v libaom-av1         
-crf 28                     //Used a value in the middle to get an average output
-b:v 3M                     
-cpu-used 4 
-g 120 
-tiles 2x2 
-threads 8 
-pix_fmt yuv420p10le 
output_av1.mkv

## -i input.mp4

Path to the input file.

## -c:v libaom-av1

Which codec to use.

## -crf <value>

The CRF value can be from 0–63. Lower values mean better quality and greater file size. 0 means lossless.

## -b:v <bitrate>

Specify the target bitrate (e.g., -b:v 3M for 3 Mbps).

## -crf <value> -b:v <bitrate>

Ensures that a constant (perceptual) quality is reached while keeping the bitrate below a specified upper bound or within a certain bound. 
The quality is determined by the -crf, and the bitrate limit by the -b:v where the bitrate MUST be non-zero.

It is also possible to specify a minimum and maximum bitrate instead of a quality target like this:
-minrate 500k -b:v 2000k -maxrate 2500k

## -cpu-used <value>

Ranges from 0 (slowest, best quality) to 8 (fastest, lower quality).

## -tiles <cols>x<rows>

Divides the video frame into tiles for parallel encoding.

## -g <frames>

The number of frames between keyframes (default ~250).

## -threads <num>

Adjust for optimal performance based on your CPU.

## -pix_fmt <value>

Specify color depth and chroma subsampling for higher fidelity.
Use yuv420p for 8-bit or yuv420p10le for 10-bit encoding.


Command:
ffmpeg -i input/input_bbb.mp4 -c:v libaom-av1 -crf 28 -b:v 3M -cpu-used 4 -g 120 -tiles 2x2 -threads 8 -pix_fmt yuv420p10le output/output_av1.mkv

ffmpeg -i input/input_bbb.mp4 -i output/output_av1.mkv -lavfi "[0:v][1:v]psnr,split[v1][v2];[v2][1:v]libvmaf" -f null -
ffmpeg -i input/input_bbb.mp4 -i output/output_av1.mkv -lavfi "[0:v][1:v]psnr=stats_file=psnr.log,split[v1][v2];[v2][1:v]libvmaf=log_path=vmaf.json;[v1]null" -f null -



PSNR y:21.849728 u:36.284207 v:37.411497 average:23.541910 min:5.320119 max:inf
VMAF score: 58.305250