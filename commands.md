# Encoder options

+ libaom        // Reference Encoder                    // Either that or
+ SVT-AV1       // New Standard Encoder by AOMedia      // this one
+ rav1e         // Xiph Encoder for AV1 (Claims to be the fastest)
+ AMD AMF AV1   // Optimized for AMD GPU's


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

By default, libaom's maximum keyframe interval is 9999 frames. This can lead to slow seeking, especially with content that has few or infrequent scene changes.

The -g option can be used to set the maximum keyframe interval. Anything up to 10 seconds is considered reasonable for most content, so for 30 frames per second content one would use -g 300, for 60 fps content -g 600, etc.

To set a fixed keyframe interval, set both -g and -keyint_min to the same value. Note that currently -keyint_min is ignored unless it's the same as -g, so the minimum keyframe interval can't be set on its own.

For intra-only output, use -g 0.

## -threads <num>

Adjust for optimal performance based on your CPU.

## -pix_fmt <value>

Specify color depth and chroma subsampling for higher fidelity.
Use yuv420p for 8-bit or yuv420p10le for 10-bit encoding.


## Command for Big Buck Bunny:
time ffmpeg -i input/input_bbb.mp4 -c:v libaom-av1 -crf 28 -b:v 3M -cpu-used 4 -g 120 -tiles 2x2 -threads 8 -pix_fmt yuv420p10le output/output_av1.mkv

Forgot to add time but it lasted circa 110min

ffmpeg -i input/input_bbb_part1.mp4 -i output/output_bbb_av1_1_3M.mkv -lavfi "[0:v][1:v]psnr=stats_file=psnr.log,split[v1][v2];[v2][1:v]libvmaf=log_path=vmaf.json;[v1]null" -f null -

PSNR y:21.849728 u:36.284207 v:37.411497 average:23.541910 min:5.320119 max:inf

VMAF score: 58.305250

## Command for Youtube cat video:

time ffmpeg -i input/input_cat.mp4 -c:v libaom-av1 -crf 28 -b:v 3M -cpu-used 4 -g 120 -tiles 2x2 -threads 8 -keyint_min 120 -pix_fmt yuv420p output/output_cat_av1.mkv

4749.10s user 51.05s system 419% cpu 19:03.62 total      ( ( ( 4749.10s + 51.05s ) / 60 ) / 4.19 ) = 19.03.62 min.  The other time is all cpus combined I guess.

ffmpeg -i input/input_cat.mp4 -i output/output_cat_av1.mkv -lavfi "[0:v][1:v]psnr=stats_file=psnr_cat.log,split[v1][v2];[v2][1:v]libvmaf=log_path=vmaf_cat.json;[v1]null" -f null -


PSNR y:27.133462 u:42.133084 v:44.437771 average:28.840180 min:11.648624 max:53.755901

VMAF score: 60.845296



aom-params