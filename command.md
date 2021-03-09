#!/bin/bash
# 格式化U盘
# 查看U盘分区
sudo fdisks -l 
# 卸载改分区（以/dev/sdb1 为例） 
sudo umount /dev/sdb1    
# 格式化，
sudo mkfs.vfat  /dev/sdb1

# !! Repeats the previous command
# !10 Repeat the 10th command from the history
# !-2 Repeat the 2nd command (from the last) from the history
# !string Repeat the command that starts with “string” from the history
# !?string Repeat the command that contains the word “string” from the history
# ^str1^str2^ Substitute str1 in the previous command with str2 and execute it
# !!:$ Gets the last argument from the previous command.
# !string:n Gets the nth argument from the command that starts with “string” from the history.
# !^ first argument of the previous command
# !$ last argument of the previous command
# !* all arguments of the previous command
# !:2 second argument of the previous command
# !:2-3 second to third arguments of the previous command
# !:2-$ second to last arguments of the previous command
# !:2* second to last arguments of the previous command
# !:2- second to next to last arguments of the previous command
# !:0 the command itself
# shell 参数高级使用
# Alt+0+.: first argument of last command
# 如!#^表示第一个参数， !#$表示最后一个参数，而!#:2表示第二个参数，所以有：
# Decide Which Command Run On Success and Fail
cp -r /path/to/a/very/long/dir0 !#^
cp -r /path/to/a/very/long/dir0 !#:2:h/dir1
# 等价于
cp -r /path/to/a/very/long/dir0 /path/to/a/very/long/dir1
mv foo/bar/poit/zoid/{narf,troz}.txt

# 从视频转gif
ffmpeg            -i test.mp4      `# 输入视频                              ` \
                  -ss 00:00:10     `# 从视频中第10秒开始                    ` \
                  -t 3             `# 截取时长3秒                           ` \
                  -s 640x360       `# GIF 的分辨率                          ` \
                  -r 15            `# 表示帧率，网上下载的视频帧率通常为 24 ` \
                  -b:v 2048k       `# 修改比特率，输出高质量gif             ` \
test.gif         &&                \
convert test.gif  -fuzz 20%        \
                  -layers Optimize \
test2.gif

# 复合字体效果
# 用朴素简单的文本来作为图像是非常无趣的，但是通过很少的工作对文本进行覆盖和染色，就可以产生一些很漂亮和奇妙的效果。
# 想要做到这一点，我们需要对文本进行多次绘制操作、覆盖不同的颜色、进行拼接、并从大量图像处理选项中找到一些合适的方法来处理文本，让它们产生更加丰富有趣的特效，而不是原来纯粹的枯燥的文字。
# 请注意，不仅仅包括我们对文本使用的字体，还有其它那些大量的处理操作很多都可以用于其它图像之中。特别是，你可以将它们用于剪贴画图像的处理。
# 平铺字体：你并不会被限制只能使用一个固定的颜色来绘制字体。而可以在字体中使用的平铺拼接图案。
convert        -font ~/.local/share/fonts/simkai.ttf `# 设置文字字体   ` \
               -size 320x100 xc:lightblue            `# 设置图像尺寸   ` \
               -pointsize 72                         `# 文字尺寸       ` \
               -tile pattern:checkerboard            `# 平铺显示       ` \
               -annotate +28+68 'Anthony'            `# 给图片添加注释 ` \
font_tile.jpg

# 请注意，上面命令中的-tile设置会覆盖任何-draw选项中-fill设置的颜色。
# wordPic2.gif 如果使用的IM v6.3.2版本，那么也可以使用-fill设置来指定一张平铺图像，但是这种用法并不推荐使用，因为许多其它选项会用到-fill设置中的颜色，但它们并不支持平铺图像，这时就会使用默认的“黑色”来代替。
# 平铺图像也可以相对于图像背景的原点进行偏移，只用在图像的-tile选项前指定-origin设置就可以实现。这样平铺图像就会按照指定的偏移量在平铺拼接时进行偏移了。
# 梯度渐变字体：平铺拼接操作不只是用于小的那部分区域，而且可以用于整个画布尺寸的区域。
convert             -font ~/.local/share/fonts/simkai.ttf \
                    -size 320x100 xc:lightblue            \
                    -pointsize 72                         \
                    -tile gradient:                       \
                    -annotate +28+68 'Anthony'            \
font_gradient.jpg

# 倒转的字体：
convert              -size 320x100 xc:lightblue            \
                     -font ~/.local/share/fonts/simkai.ttf \
                     -pointsize 72                         \
                     -fill Navy                            \
                     -annotate 180x180+300+35 'Anthony'    \
font_upsidedown.jpg

# 实心阴影：对文本进行两次绘制操作，并且加入一定的偏移量，你就可以生产最简单的实心阴影边缘效果了。
convert         -size 320x100 xc:lightblue            \
                -font ~/.local/share/fonts/simkai.ttf \
                -pointsize 72                         \
                -fill black                           \
                -draw "text 28,68 'Anthony'"          \
                -fill white                           \
                -draw "text 25,65 'Anthony'"          \
font_shadow.jpg

# 倾斜的阴影：因为注释（-annotate）字体绘制选项可以在垂直维度和水平维度上分别进行选择操作，所以你就可以为字体指定一些奇怪的旋转方式，如“回转”或“倾斜”等。这为我们生成怪异奇妙的阴影或使自己的字体变为斜体或倾斜状态提供了极大的方便。
convert         -size 320x115 xc:lightblue            \
                -font ~/.local/share/fonts/simkai.ttf \
                -pointsize 72                         \
                -fill Navy                            \
                -annotate 0x0+12+55   'Anthony'       \
                -fill RoyalBlue                       \
                -annotate 0x130+25+80 'Anthony'       \
font_slewed.jpg

# 可以参考一张表格，其中总结了文本旋转的效果，见Annotate Text Option。
# 当然“Candice”字体并不是最适合这种阴影效果的方案，我们可能还需要添加其它一些细节效果来使结果看起来更加具有3D图像的感觉。例如你可以参考Blurring the shadow with distance。
# 倾斜字体：你还可以使用
# -draw选项来实现字体倾斜，虽然这种方法有点麻烦，因为它涉及到额外的MVG（Magick矢量图形）的操作，要将绘制图像的表面进行扭曲。因为表面要被扭曲，那么我们在扭曲之前使用“translate”来设置字体的位置将会是个好主意。
convert             -size 320x100 xc:lightblue                           \
                    -font ~/.local/share/fonts/simkai.ttf                \
                    -pointsize 72                                        \
                    -fill Navy                                           \
                    -draw "translate 28,68 skewX -20 text 0,0 'Anthony'" \
font_slanted.jpg

# 可以参考一张表格，其中总结了文本旋转的效果，见Annotate Text Option。
# 当然“Candice”字体并不是最适合这种阴影效果的方案，我们可能还需要添加其它一些细节效果来使结果看起来更加具有3D图像的感觉。如果你发现了其它有趣的效果，那么请告诉我们，这样就可以与世界其它的爱好者一同分享你的成果了。
# wordPic8.gif “
# -annotate”和“
# -draw skew”命令实际上都可以完成绘图表面X和Y轴的旋转操作。并且效果和在现有的图像上使用“
# -shear”选项不同，它们会延长图像的倾斜轴，所以使用这些选项不会导致图像的高度（或宽度）发生改变。
# 印版字体：对字体进行三次绘制操作，分别用较深的颜色、较浅的颜色和原来的颜色，就可以实现印版一样的三维效果。
convert        -size 320x100 xc:lightblue            \
               -font ~/.local/share/fonts/simkai.ttf \
               -pointsize 72                         \
               -fill black                           \
               -annotate +24+64 'Anthony'            \
               -fill white                           \
               -annotate +26+66 'Anthony'            \
               -fill lightblue                       \
               -annotate +25+65 'Anthony'            \
font_stamp.jpg

# 请注意最后一步文本绘制操作是如何将字体的中间部分删除的。另外，这种方法只能在纯色背景中晚餐，请参阅Using a Mask Image，看看我们如何在随机的背景下完成这种效果，而不是纯色背景。
# 如果你交换两种颜色的位置，就可以得到一个向上凸出的字体效果，而不是一个向下凹陷的字体效果。
# 堆叠或三维块字体：可以通过重复多次进行字体绘制并加入偏移量来生成。
convert          -size 320x100 xc:lightblue            \
                 -font ~/.local/share/fonts/simkai.ttf \
                 -pointsize 72                         \
                 -fill gray                            \
                 -annotate +29+69 'Anthony'            \
                 -annotate +28+68 'Anthony'            \
                 -annotate +27+67 'Anthony'            \
                 -annotate +26+66 'Anthony'            \
                 -annotate +25+65 'Anthony'            \
                 -annotate +24+64 'Anthony'            \
                 -fill navy                            \
                 -annotate +23+63 'Anthony'            \
font_extrude.jpg

# 请注意，这不是一个简单的阴影效果，而是让绘制的字体呈现出适当的厚度。
# 这种方法就是不断的重复绘制，并且不仅可以对文本使用，还可以用于任何具有形状的图像。你还可以参考另一个例子Adding Thickness to a Thumbnail。
# 轮廓字体：我们可以对多个绘制的文本使用小位置的偏移量，来创建文本字体的轮廓效果。
convert           -size 320x100 xc:lightblue            \
                  -font ~/.local/share/fonts/simkai.ttf \
                  -pointsize 72                         \
                  -fill black                           \
                  -annotate +24+64 'Anthony'            \
                  -annotate +26+64 'Anthony'            \
                  -annotate +26+66 'Anthony'            \
                  -annotate +24+66 'Anthony'            \
                  -fill white                           \
                  -annotate +25+65 'Anthony'            \
font_outlined.jpg

# 这种方法也需要进行多次重复操作，而且并不是产生轮廓效果的最好解决方案。
# 因为ImageMagick程序还允许你使用
# -stroke选项来绘制字体的轮廓，所以我们可以有更好的解决方案。（见下面的
# -stroke字体）。
# 无论如何像这样的多次重绘操作，用来为预先准备的剪贴画图像生成轮廓是非常方便的，并且你可以在互联网上找到很多这样的图像。并且这项技术对于其它不能使用
# -stroke选项的图形库和处理程序（如PHP中的GD程序等）也是非常有用的。
# 另一个选择这种轮廓风格用于显示的原因是，如果对于非常尖锐的字体，那么这种轮廓显示结果可能会更好。
# 例如，我们在这里进行了9次文本绘制，来显示字体中的尖锐点。并且把轮廓绘制得粗一些。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-draw "fill black text 27,67 'Anthony'  \
text 25,68 'Anthony' \
text 23,67 'Anthony' \
text 22,65 'Anthony' \
text 23,63 'Anthony' \
text 25,62 'Anthony' \
text 27,63 'Anthony' \
text 28,65 'Anthony' \
fill white text 25,65 'Anthony' " \
font_outlined_12.jpg

# 你同时也可以注意到
# -fill选项设置的颜色，也可以在
# -draw选项中进行修改。
# 多色轮廓：这种技术非常有用的原因就是，你可以不用在绘制字体轮廓时被限制只使用一种颜色。我们非常精心的设计了绘制流程，一共对字体仔细地进行了12次重绘操作并且使用了5中不同的颜色，才生成下面的彩色凸出状效果的字体，并进行了一些边缘颜色平滑操作。
convert -size 320x100 xc:lightblue  \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72 \
-gravity center  \
-draw "fill navy         text  2,2  'Anthony'  \
fill navy         text  0,3  'Anthony' \
fill navy         text  3,0  'Anthony' \
fill dodgerblue   text  0,2  'Anthony' \
fill dodgerblue   text  2,0  'Anthony' \
fill dodgerblue   text 
-2,2  'Anthony'  \
fill dodgerblue   text  2,
-2 'Anthony'  \
fill lavender     text 
-2, \
-2 'Anthony'  \
fill lavender     text  0,
-3 'Anthony'  \
fill lavender     text 
-3,0  'Anthony'  \
fill skyblue      text  0,
-2 'Anthony'  \
fill skyblue      text 
-2,0  'Anthony'  \
fill blue         text  0,0  'Anthony' " \
font_colourful.jpg

# 其实还有更好的方法可以用来创建这样一个凸出效果的字体，而且这个工作非常简单，只需要使用几种颜色，而不是整个范围的颜色。
# 字体轮廓（Stroke）：
# -stroke选项设置允许你直接绘制字体的轮廓。但是通常情况下，轮廓的颜色都设置为无色（none），所以就像没有使用一样。而轮廓的宽度可以使用
# -strokewidth选项来变化，它的默认值为1。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill white \
-stroke black \
-annotate +25+65 'Anthony'  \
font_stroke.jpg

# 下面是将边框宽度设置为3的一个例子。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill white \
-stroke black \
-strokewidth 3  \
-annotate +25+65 'Anthony' \
font_stroke_3.jpg 

# 请注意，其中轮廓的颜色不仅会蚕食（遮蔽）字体外面的背景颜色，同时也会蚕食字体内部的颜色。了解更多详细信息，请参阅Stroke and Stroke Width Options。
# 加宽的轮廓：通过第二次重绘字体，并且没有开启
# -stroke选项，那么线条内部的部分将被删除，创建出一个更漂亮的加宽字体轮廓。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72 \
-fill white  \
-stroke black \
-strokewidth 5 \
-annotate +25+65 'Anthony'  \
-stroke none \
-annotate +25+65 'Anthony'  \
font_stroke_thick.jpg

# 我们甚至还可以进一步使用
# -stroke选项，在Stroke and StrokeWidth Options中将会更深入地探讨这个选项。
# 细的轮廓：通过关闭填充（
# -fill）颜色，你可以仅仅只在图像中留下字体的轮廓。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill none \
-stroke black \
-annotate +25+65 'Anthony'  \
font_stroke_thin.jpg

# 双重轮廓：通过重绘文本并使用多个不同的轮廓宽度，可以产生出双重轮廓的效果！第一次文本绘制，可以使用任何填充颜色来填充字体里面的部分，或者使用无色（none）就像我们上面那样只留下背景色。但是，最后一次的文本重绘操作的填充颜色必须设置为无色（none），否则就会无法正常工作。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill none \
-stroke black \
-strokewidth 3 \
-annotate +25+65 'Anthony'  \
-fill none \
-stroke white \
-strokewidth 1 \
-annotate +25+65 'Anthony'  \
font_stroke_double.jpg

# 与前面介绍的印版字体不同的是，上面这种方法不要求中间部分的字体被删除。因此，这种方法可以在任何背景上工作，而不会产生其它的问题。
# 迷幻字体：通过与上面类似的重绘方式，慢慢地减少轮廓的宽度，然后不断交换颜色，就可以很容易地生成迷幻轮廓效果了。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72 \
-fill white  \
-stroke black \
-strokewidth 25 \
-annotate +25+65 'Anthony'  \
-stroke white \
-strokewidth 20 \
-annotate +25+65 'Anthony'  \
-stroke black \
-strokewidth 15 \
-annotate +25+65 'Anthony'  \
-stroke white \
-strokewidth 10 \
-annotate +25+65 'Anthony'  \
-stroke black \
-strokewidth  5 \
-annotate +25+65 'Anthony'  \
-stroke none \
-annotate +25+65 'Anthony'  \
font_psychedelic.jpg

# 你还可以让它更加的迷幻，通过使用互相冲突的颜色，不同的宽度间隔，甚至让文字在中心附近做一些小的位置移动。尽管去实验，看看你能想出什么奇妙的效果。
# 气球效应：下面我进行了与上面“加粗轮廓字体”完全一样的操作，但是纯粹由于意外，在重新绘制字体时我使用了白色作为轮廓颜色。这导致了一个有趣的字体放大效果，并且仍然有加粗的轮廓。这种“浮肿”的字体效果，仿佛像一个充满气的气球。
# 这表明如果你努力去实验，总是会有所回报。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill black \
-stroke black \
-strokewidth 5 \
-annotate +25+65 'Anthony'  \
-fill white \
-stroke white \
-strokewidth 1 \
-annotate +25+65 'Anthony'  \
font_balloon.jpg

# 连接的字体：通过对
# -kerning选项进行一些小的负数的设置（在IM v6.4.7
# -10版本加入的），并且绘制相同的字体两次（像前面的例子一样），你就可能会得到加粗字体的所有字符连接在一起的效果，从而产生了一个有趣的变化。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-kerning \
-6 \
-strokewidth 4 \
-fill white  \
-stroke black \
-annotate +28+68 Anthony  \
-stroke none \
-annotate +28+68 Anthony  \
font_joined.jpg

# 重叠的字体：产生重叠的字体效果其实只需要进行一点点变化，就是分别单独的绘制每一个字符，这样就可以生成每个字符覆盖到前面字符上的变化效果了。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-stroke black \
-strokewidth 4 \
-fill white  \
-stroke black \
-annotate  +28+68 A \
-stroke none \
-annotate  +28+68 A  \
-stroke black \
-annotate  +90+68 n \
-stroke none \
-annotate  +90+68 n  \
-stroke black \
-annotate +120+68 t \
-stroke none \
-annotate +120+68 t  \
-stroke black \
-annotate +138+68 h \
-stroke none \
-annotate +138+68 h  \
-stroke black \
-annotate +168+68 o \
-stroke none \
-annotate +168+68 o  \
-stroke black \
-annotate +193+68 n \
-stroke none \
-annotate +193+68 n  \
-stroke black \
-annotate +223+68 y \
-stroke none \
-annotate +223+68 y  \
font_overlapped.jpg

# 但是，这种方法就需要你（手动或使用自动脚本）为每个字符确定适当的位置。每个字符本身的宽度可以通过生成标签或字母来确定，而不需要进行任何的
# -strokewidth设置。请参阅Determining Font Metrics中的例子。
# 注意，并不像使用
# -kerning设置（前面的例子）中简单的固定值来确定每个字符的位置，上面的方法在生成每个字母时就设置了不同的坐标值。例如，“t”和“h”字母之间只有一点点重叠，而在“n”和“y”字母之间则有更大的重叠。
# 上下移动的字体：如果你使用绘制单个字符（重叠或者不重叠）进行更深入的发挥，那么你还可以让它们产生抖动或随机模式的效果，尤其是使用不同的上下偏移设定。
# 你甚至可以在此基础上发挥到极致，以产生出特殊的效果，例如：
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-stroke black \
-strokewidth 4 \
-fill white  \
-stroke black \
-annotate  +26+80 A \
-stroke none \
-annotate  +26+80 A  \
-stroke black \
-annotate  +95+63 n \
-stroke none \
-annotate  +95+63 n  \
-stroke black \
-annotate +133+54 t \
-stroke none \
-annotate +133+54 t  \
-stroke black \
-annotate +156+67 h \
-stroke none \
-annotate +156+67 h  \
-stroke black \
-annotate +193+59 o \
-stroke none \
-annotate +193+59 o  \
-stroke black \
-annotate +225+59 n \
-stroke none \
-annotate +225+59 n  \
-stroke black \
-annotate +266+54 y \
-stroke none \
-annotate +266+54 y  \
font_jittered.jpg

# 模糊的字体：对字体使用
# -blur模糊选项可以产生字体颜色直线蔓延的效果。这个选项能让你把图像的颜色向各个方向发散。这就可以让你产生出柔和的阴影，或者喷漆状的效果。下面的例子表明，你可以使用
# -blur选项实现这个效果。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill navy \
-annotate +25+65 'Anthony'  \
-blur 0x3   \
font_fuzzy.jpg 

# 请注意，当你使用
# -blur选项时，它将会作用于整个图像。如果你想使用一张现有的图像进行文字模糊，你将不得不单独绘制字体（在透明背景上），然后再将它覆盖到背景图像中。
# wordPic25.gif 
# -blur（或
# -gaussian）选项可能会修改一个比你所希望的面积更大的区域。如果你的背景画布不够大，那么可能会在使用这些选项时返回错误。如果图像中发生了增加额外空间的情况，那么就得使用
# -border选项，或对其设置工作半径限制（选项的第一个参数）。
# 同时，模糊图像操作一般会使它后面的修剪（
# -trim）选项不起什么效果。只要你对图像使用了模糊操作，通常需要进行手动修剪或其它可能需要的调整。
# 模糊阴影：使用模糊的字体作为阴影并设置一定的偏移量。请注意，我们特意使用了一个较大的模糊值。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-annotate +30+70 'Anthony' \
-blur 0x4  \
-fill white \
-stroke black \
-annotate +25+65 'Anthony'  \
font_shadow_fuzzy.jpg

# 柔和的阴影： -shadow选项不仅将允许你在含有透明度的图像中生成并定位柔和的模糊阴影，而且还允许你使用任何颜色，并设置普通的透明度水平。
convert -size 300x100 xc:none \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill white \
-stroke black \
-annotate +25+65 'Anthony' +clone \
-background navy \
-shadow 70x4+5+5 +swap \
-background lightblue \
-flatten \
-trim +repage \
font_shadow_soft.jpg 

# 关于 -shadow选项的更多信息，请参阅Generating Shadows。
# 对于IM v6.3.1版本，“montage”命令也可以在包含透明度的图像中生成柔和的阴影图形。这意味着你可以很容易的在图像中添加带阴影的标签（label:）。
montage -background none \
-fill white \
-font ~/.local/share/fonts/simkai.ttf  \
-pointsize 72 label:'Anthony' +set label  \
-shadow \
-background lightblue \
-geometry +5+5  \
font_montage_shadow.jpg

# 但是，你不能对montage命令生成的阴影进行任何偏移量、颜色或阴影模糊程度的控制。
# 柔和的轮廓：使用模糊的字体作为轮廓边框。这就像是使用原始字体的板子挡在喷枪前面形成的效果。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-annotate +25+65 'Anthony' \
-blur 0x5  \
-fill white \
-annotate +25+65 'Anthony'  \
font_outline_soft.jpg 

# 需要注意的是字体边缘颜色很浅，因为不仅是字体的黑色进行了发散，而背景颜色也向里面进行了发散，所以边缘颜色中只有50％的黑色。
# 解决这个问题的方法之一就是使用阴影轮廓，采用
# -level选项来调整与修复图像亮度，虽然其中使用了一些非常先进的图像处理技术。
# 颜色更浓的柔和阴影：解决柔和轮廓边缘较浅的另一种方式就是对具有很宽轮廓的字体进行模糊。这将有效地让50％黑色的模糊点进一步远离字体的边缘。它甚至还可以允许使用更大的模糊值让黑色进一步蔓延发散。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-stroke black \
-strokewidth 8 \
-annotate +25+65 'Anthony' \
-blur 0x8  \
-fill white \
-stroke none \
-annotate +25+65 'Anthony'  \
font_denser_soft_outline.jpg

# 使用这种方法生成的一个实际例子请参考Adding image labels to thumbnails，另外最后一个例子参考Annotating on Top of Images。
# 随距离变化的模糊阴影：通过介绍可变模糊映射，你现在可以使阴影的模糊程度根据阴影到字体的距离而发生变化。
# 例如在这里，我使用倾斜的字体阴影，然后对阴影进行模糊操作，让它在上端并不模糊而在底部更加的模糊。
convert -size 320x40 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill RoyalBlue \
-annotate 0x125+20+0 'Anthony'  \
-size 320x45 gradient:black-append \
-compose Blur \
-set option:compose:args 20x5+45 \
-composite  \
-size 320x60 xc:lightblue \
-fill Navy \
-annotate 0x0+20+59 ′Anthony′ 
+swap -append   \
font_var_blur.jpg 

# 请注意，我不仅仅是使用了一个循环模糊，因为当光线照射到一个倾斜的表面上时会形成椭圆形而不是圆形。所以这种模糊效果也需要形成椭圆形。基本上，我使用了一个椭圆形的模糊变化来达到这种效果。
# 最后一点，使用注释（
# -annotate）选项中的角度参数来创建倾斜的文本（见
# -annotate选项的参数用法），可能不是产生这样的初始三维阴影的最好办法。基本上它不能让阴影伸长或缩短，就像一个真正的阴影那样，因为它仅仅只能对阴影进行旋转和倾斜。
# 另一个更好的方法就是使用3个点的仿射（Affine）扭曲，可以让你更好地控制阴影的位置（参见3d Shadows, using Affine Shears）。当然，你仍然需要使用渐变模糊的技术，让图像看起来是合理的。
# 带有斜面的字体：
# -shade选项可以用来产生非常漂亮的3D字体斜面和平滑弯曲的边缘。
convert -size 320x100 xc:black \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill white \
-annotate +25+65 'Anthony'  \
-shade 140x60  \
font_beveled.jpg 

# 这比印版字体看起来效果更好，但是
# -shade选项只能产生灰度图像。但另一方面，也还有很多方法可以替换上述灰度图像的结果，不论你需要什么样的颜色。
# 使用阴影来为字体产生斜面效果的最大问题就是其中斜角的宽度并不是可以调节的。它基本上是一个大约5像素宽的固定值，无论你使用的字体尺寸是多少。
# 锥形字体：通过采用新的形态距离方法（在IM v6.6.2版本中加入），并结合
# -shade选项生成的阴影，你就可以使整个字体看起来呈现一个三维山脊（锥形）的效果了。
# 但是这还需要对形状中的抗锯齿像素进行一些特殊的处理，最后的效果就是产生一个山脊般的锥形字体。
convert -size 320x100 xc:black \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill white \
-annotate +25+65 'Anthony'  \
-gamma 2  +level 0,1000 \
-white-threshold 999  \
-morphology Distance:-1 Euclidean:4,1000 \
-auto-level  \
-shade 135x30 \
-auto-level +level 10,90% \
font_conic.jpg 

# 通过加入自适应模糊（
# -adaptive
# -blur）选项，你就可以对上述结果图像进行平滑处理，得到一个更漂亮并有奇怪光泽外观的字体了。
convert -size 320x100 xc:black \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill white \
-annotate +25+65 'Anthony'  \
-gamma 2  +level 0,1000 \
-white-threshold 999  \
-morphology Distance:-1 Euclidean:4,1000 \
-auto-level  \
-shade 135x30 \
-auto-level +level 10,90%  \
-adaptive-blur 0x2  \
font_conic_smoothed.jpg 


# -adaptive
# -blur）选项移动到
# -shade选项前使用，将导致字体的边缘被模糊处理，而不是字体形状的中央脊（骨架）。产生的结果就像是一个锋利的脊状字体压到橡胶板上的样子。
convert -size 320x100 xc:black \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill white \
-annotate +25+65 'Anthony'  \
-gamma 2  +level 0,1000 \
-white-threshold 999  \
-morphology Distance:-1 Euclidean:4,1000 \
-auto-level  \
-adaptive-blur 0x2  \
-shade 135x30 \
-auto-level +level 10,90%  \
font_conic_ridge.jpg 

# 使用另一种不同的距离内核，如切比雪夫（Chebyshev），将会在外观更整齐的字体中产生更好的效果，如Arial系列的字体。
convert -size 320x100 xc:black \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 70  \
-fill white \
-annotate +5+70 'Anthony'  \
-gamma 2  +level 0,1000 \
-white-threshold 999  \
-morphology Distance:-1 Chebyshev:1,1000 \
-auto-level  \
-shade 135x30 \
-auto-level +level 10,90% \
font_chebyshev.jpg 

# 内锥字体：当然，你不必一定将距离函数应用到整个字体中，而是也可以将它仅仅限制在边缘部分，只对字体内边缘产生锥形效果。
convert -size 320x100 xc:black \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill white \
-annotate +25+65 'Anthony'  \
-gamma 2 +level 0,1000 \
-white-threshold 999  \
-morphology Distance:5 Euclidean:4,1000 \
-level 0,5000  \
-shade 135x30 \
-auto-level +level 10,90% \
font_inner_bevel.jpg 

# 拱形的字体：使用
# -wave选项（见详情Sine Wave Displacement）可以将图像的像素进行垂直转移形成一个拱形。而垂直的形状将仍然保持垂直状态，绘制的字符将会被倾斜产生出拱形曲线。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill navy \
-annotate +25+65 'Anthony'  \
-background lightblue \
-wave \
-50x640 \
-crop x110+0+10  \
font_wavy.jpg

# 请注意，如果使用
# -wave选项来创建一个拱形，你需要使用的波长应该是图像宽度的两倍（2x320或640像素）。同时，因为
# -wave选项还可能根据设定的振幅值在图像上增加空白区域，所以在使用之后应该对图像进行修改或裁剪去除这些部分。
# 这是使文本成为拱形的一个简单、快速而且有效的方法。
# 弧形字体：其实最普通的
# -distort选项也提供了使文本/图像变形的其它方法。例如其中的“Arc”方法就可以使字体成为真正的弯曲圆弧状，而不仅仅是前面例子中垂直转移的拱形。
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill navy \
-annotate +25+65 'Anthony'  \
-distort Arc 120 \
-trim +repage  \
-bordercolor lightblue \
-border 10  \
font_arc.jpg 

# 环形字体：你甚至还可以把它这种方法用得更极端一些，将文本扭曲为一个完整的或近似完整的圆。
convert -font ~/.local/share/fonts/simkai.ttf \
-pointsize 32 \
-background lightblue  \
-fill navy label:"Anthony's IM Examples"  \
-virtual-pixel background \
-distort Arc 340  \
font_circle.jpg

# 参考Arc Distortion，还可以看到更多的可能和介绍。
# 螺旋状字体：通过在弯曲字体前添加一点旋转，让字体倾斜一个角度，就可以把环形转换成螺旋状的了。
convert -font ~/.local/share/fonts/simkai.ttf \
-pointsize 32 \
-background lightblue  \
-fill navy  label:"Anthony's IM Examples"  \
-rotate 12 \
-virtual-pixel background \
-distort Arc 360  \
-trim \
-bordercolor lightblue \
-border 5x5  \
font_spiral.jpg 

# 但是，图像中文字的高度（径向）仍然保持不变，它没有被拉伸或压缩，只是当它的字符越靠近中心时，就生产了越强的扭曲。你也可以解决这个问题，只要在应用Arc方法扭曲文本之前，使用perspective扭曲方法作为文字旋转操作的一部分来调整字体的高度就可以了。
# 使用这种方法的问题就是，你只能进行一圈螺旋旋转，但是通过采用多条线，并将它们小心的连接起来，你就可以生成多个螺旋了。
# 如果你进行了尝试，希望能提交给我一个例子？
# 颤抖状的字体：我们在上面的拱形字体中使用的
# -wave选项，也可以设置为更高的频率和较小的幅度，生成颤抖状的字体。另外，还可以增加一些旋转设置，甚至能产生出任何你喜欢角度的振动！
convert -size 320x100 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill navy \
-annotate +25+65 'Anthony'  \
-background lightblue \
-rotate 85 \
-wave 2x5 \
-rotate \
-85  \
-gravity center \
-crop 320x100+0+0 +repage \
font_vibrato.jpg 

# 如果需要更多的使用
# -distort选项的信息，可以参看Warping Images，尤其是Wave Distortion Operator。
# 彗星状的字体：专用模糊操作选项之一的运动模糊（
# -motion
# -blur）选项可以让你在图像中创建出一个拖着尾巴像彗星状的字体。
convert -size 340x120 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill navy \
-annotate +45+95 'Anthony' \
-motion-blur 0x25+65  \
-fill black \
-annotate +45+95 'Anthony' \
-motion-blur 0x1+65  \
font_comet.jpg

# 你还可以采用不同的颜色让这个复合字体变得栩栩如生，就像一个真正的喷火的彗星似的。
# 你也可以使用专用模糊选项做更多的事情，但是IM程序中的这些选项仍然处于试验阶段，在不久的将来它们的用法可能会发生改变。
# 冒烟状的字体：与
# -wave选项结合使用，可以使彗星字体外观变得像烟雾、气味甚至上升的火焰状的字体！
convert -size 320x120 xc:lightblue \
-font ~/.local/share/fonts/simkai.ttf \
-pointsize 72  \
-fill black \
-annotate +25+95 'Anthony' \
-motion-blur 0x25+90  \
-background lightblue \
-rotate 60 \
-wave 3x35 \
-rotate -60  \
-gravity center \
-crop 320x120+0+0 +repage +gravity  \
-fill navy \
-annotate +25+95 'Anthony'   \
font_smoking.jpg 

# 将1,1坐标得颜色设置为透明
convert sig.jpg  -alpha set                \
                 -channel RGBA             \
                 -fuzz 10%                 \
                 -fill "rgb(255,255,255)"  \
                 -draw "color 1,1 replace" \
                 -transparent white        \
sig10.png

# 拼图
montage ./*.png  -tile 50x20     `#  x轴50个 y轴 20个 ` \
                 -geometry 32x32 `# 长宽 各32         ` \
icons.png

# 拼图
convert image1.png image2.png image3.png -gravity south +append result.png

# 1、图片指定区域变色
convert old.png -region 50x60+20+10  `# 指定区域尺寸和起始坐标（50x60+20+10） ` \
                -fill "rgb(255,0,0)" `# 区域颜色（rgb(255,0,0)）              ` \
                -colorize 20%        `# 着色程度（20%）                       ` \
new.png

# 图片指定区域变色（放大或缩小区域）
convert old.png  -region 50x60+20+10  `#指定区域尺寸和起始坐标   ` \
                 -resize 120%         `#尺寸（120%）             ` \
                 -fill "rgb(255,0,0)" `#区域颜色（rgb(255,0,0)） ` \
                 -colorize 20%        `#着色程度（20%）          ` \
new.png

# 替换相同颜色的区域（指定颜色）
convert old.jpg -alpha set                 `#                         ` \
                -channel RGBA              `#                         ` \
                -fuzz 10%                  `# 指定颜色差异程度（10%） ` \
                -fill "rgb(0,0,0)"         `# 替换颜色（黑色）        ` \
                -opaque "rgb(255,255,255)" `# 被替换颜色（白色）      ` \
new.png

# 替换不同颜色的区域（指定颜色）将整张图片中除指定颜色外的颜色全部替换。
convert old.jpg -alpha set                                      `#                                      ` \
                -channel RGBA                                   `#                                      ` \
                -fuzz 50%                                       `# 指定颜色差异程度（50%），            ` \
                -fill "rgb(255,255,255)" +opaque "rgb(0,0,255)" `# 替换颜色（白色），指定颜色（蓝色）。 ` \
new.png

# 替换相同颜色的区域（指定坐标）
convert old.jpg -alpha set                    `#                         ` \
                -channel RGBA                 `#                         ` \
                -fuzz 10%                     `# 指定颜色差异程度（10%） ` \
                -fill "rgb(255,0,0)"          `# 替换颜色（红色）        ` \
                -draw "color 180,150 replace" `# 指定坐标（180,150）     ` \
new.png


# 替换相同颜色的连通区域（指定坐标） 。将图片连通区域中与指定位置颜色相同的颜色全部替换。
convert old.jpg -alpha set                    `#                         ` \
                -channel RGBA                 `#                         ` \
                -fuzz 10%                     `# 指定颜色差异程度（10%） ` \
                -fill "rgb(255,0,0)"          `# 替换颜色（红色）      ` \
                -draw "color 180,150 replace" `# 指定坐标（180,150）   ` \
new.png

# 图像边缘突出显示 指定探测半径（5），对图像中类似边缘的像素进行探测。使边缘突显出来，易于观察。
convert old.png -edge 5 new.png

# 8、图像边缘探测 
convert old.jpg   -background white       `#                     ` \
                  -flatten                `#                     ` \
                  -colorspace gray        `#                     ` \
                  -negate                 `#                     ` \
                  -edge 5                 `# 指定探测半径（5）。 ` \
                  -negate                 `#                     ` \
                  -normalize              `#                     ` \
                  -threshold 50%          `#                     ` \
                  -despeckle              `#                     ` \
                  -blur 0x.5              `#                     ` \
                  -contrast-stretch 0x50% `#                     ` \
new.png

# 合并图片
composite -gravity north src.jpg coverback.jpg des.jpg
convert image.png -compose over overlay.png -composite newimage.png
convert image.png -define png:format=png32  image32.png
convert image32.png -channel alpha -fx "0.5" imagealpha.png

其中src.jpg为前景图片
coverback.jpg为背景图片。
des.jpg为叠加后的结果

# 生成 ico
convert logo.png -background none favicon.ico

# 获取图片信息
identify image.png
identify -format "%wx%h" image.png  //只获取宽高


# 放大缩小
convert image.png -resize 200x200 resize.png
convert image.png -resize 50% resize.png
# //用来生成缩略图最合适
convert image.png -sample 50% sample.png   
# //处理马赛克
convert image.png -sample 10% -sample 1000% sample.png 

# 裁剪 -crop
convert image.png -crop 100x100+50+50 `#从（50，50）位置开始，裁剪一个100X100大小的图片` \
crop.png 
convert image.png -crop 100x100 crop.png
convert image.png -gravity northeast `# gravity即指定坐标原点，northwest：左上角，north：上边中间， northeast：右上角， east：右边中间 ` \
                  -crop 100x100+0+0  `#                                                                                                ` \
crop.png


# 旋转 -rotate
convert image.png -rotate 45 rotate.png

# 去除水印
ffmpeg  -irst.mp4                               `# ` \
        -vf delogo=x=15:y=35:w=100:h=20:band=20 `# ` \
out.mp4

# # 杀掉查询到的进程          
ps -ef | grep "python"|grep -v grep|cut -c 9-15|xargs kill -9
# # 重定向已经运行的程序输出  
sudo reredirect -m stdout.log `jobs -p % `
# # 保存程序输出和错误到文件  
# \C-e 3>&1 | tee /tmp/stdout-stderr.txt"                                                                        
# # 查找文件                  
# \C-afind . -iname \047*\C-e*\047\C-a\ef\eOC\e[C"                                                               
# # 文本查找                  
# \C-agrep -rn \047\C-e\047 ."                                                                                   
# # 文本命令查找              
# \C-agrep -n -A 1 \047\C-e\047 ~/.command"                                                                      
# # gr 可视化替换             
# grep -rl 'pattern'  -exec vim -c '%s/PATTERN/REPLACEMENT/gc' -c 'wq' {} \134;"                                 
# # gr 可视化替换             
# find . -type f -name '*'  -exec vim -c '%s/PATTERN/REPLACEMENT/gc' -c 'wq' {} \134;"                                                       
# \C-agrep -rl \047\C-e\047 . | xargs sed -i \047s/old/new/g\047"                                                
# # cc 复制这行到自定义粘贴板 
# \C-aecho \047\C-e\047 |tee -a /tmp/clipboard.txt | xsel -b"                                                    
# # cc 复制这行到自定义粘贴板 
# \C-aecho \047\C-e\047 >> ~/.command"                                                                           
# # 贴贴命令                  
# \C-a`grep -A 1 \047\C-e\047 ~/.command | tac`\e\C-e"                                                           
# # 贴贴第多少行命令          
# \C-a`sed -n \047\C-e,+1p\047 ~/.command | tac `\e\C-e"                                                         
# # 贴贴粘贴板                
# `awk \047END {print}\047 /tmp/clipboard.txt`"                                                                  
# # 贴贴系统粘贴板            
# `xsel -b`\e\C-e"                                                                                               
# # 贴贴多少行                
# `sed -n '$,$p' /tmp/www`"                                                                                      
# # 贴贴匹配                  
# sed -n '/.*name/p' /tmp/www"                                                                                     
# # 贴贴多少行              
# `sed -n '$,$p' /tmp/clipboard.txt`"                                                                            
# # 最后个文件夹              
# ls -d -t */ "                                                                                                  
# # 最后一个文件              
# ls -pa | grep -v / "                                                                                           
# # 打印日期                  
# `date +%Y%m%d`\e\C-e"                                                                                          
# # 打印日期时间              
# `date +%Y%m%d%H%M%S`\e\C-e"                                                                                    
# # 打印时间                  
# `date +%H%M%S`\e\C-e"                                                                                          
# # 数值计算                  
# echo 'scale=4; 10/3 ' | bc\eb\eb\C-f"                                                                          
# # 在其他主机上运行          
# \C-e\047\C-assh dc@hadoop \047source /etc/profile;\e\C-] \e\C-] "                     

vboxmanage list vms
vboxmanage list runningvms
# 开机
vboxmanage startvm  UServer01 UServer11 UServer12 UServer13 --type headless
# 关机
vboxmanage controlvm UServer01 acpipowerbutton;
^01^11^;
^11^12^; 
^01^11^; 
vboxmanage controlvm UServer01 acpipowerbutton; \
vboxmanage controlvm UServer11 acpipowerbutton; \
vboxmanage controlvm UServer12 acpipowerbutton; \
vboxmanage controlvm UServer13 acpipowerbutton;

# 主机模式网路
vboxmanage list hostonlyifs

# 主机模式
VBoxManage hostonlyif       ipconfig <name>
                            [--dhcp |
                            --ip<ipv4> [--netmask<ipv4> (def: 255.255.255.0)] |
                            --ipv6<ipv6> [--netmasklengthv6<length> (def: 64)]]
                            create |
                            remove <name>
vboxmanage hostonlyif create
# 主机模式网路
vboxmanage hostonlyif remove vboxnet0
# 主机模式网络设置
vboxmanage hostonlyif ipconfig vboxnet0  --ip 192.168.56.1       `# ` \
                                         --netmask 255.255.255.0 `# `
# 使能dhcp
vboxmanage dhcpserver add  --ifname vboxnet0        `# ` \
                           --ip 192.168.56.1        `# ` \
                           --netmask 255.255.255.0  `# ` \
                           --lowerip 192.168.56.100 `# ` \
                           --upperip 192.168.56.200 `# ` \

# vboxmanage dhcpserver modify --ifname vboxnet0 --enable
# You will need to know which ports on the guest the service uses and to decide which ports to use on the host. You may want to use the same ports on the guest and on the host. You can use any ports on the host which are not already in use by a service. For example, to set up incoming NAT connections to an ssh server in the guest, use the following command:
VBoxManage modifyvm "VM name" --natpf1 "guestssh,tcp,,2222,,22"
# In the above example, all TCP traffic arriving on port 2222 on any host interface will be forwarded to port 22 in the guest. The protocol name tcp is a mandatory attribute defining which protocol should be used for forwarding, udp could also be used. The name guestssh is purely descriptive and will be auto-generated if omitted. The number after --natpf denotes the network card, as with other VBoxManage commands.
# To remove this forwarding rule, use the following command:
VBoxManage modifyvm "VM name" --natpf1 delete "guestssh"
# If for some reason the guest uses a static assigned IP address not leased from the built-in DHCP server, it is required to specify the guest IP when registering the forwarding rule, as follows:
VBoxManage modifyvm "VM name" --natpf1 "guestssh,tcp,,2222,10.0.2.19,22"
# This example is identical to the previous one, except that the NAT engine is being told that the guest can be found at the 10.0.2.19 address.
# To forward all incoming traffic from a specific host interface to the guest, specify the IP of that host interface as follows:
VBoxManage modifyvm "VM name" --natpf1 "guestssh,tcp,127.0.0.1,2222,,22"
# This example forwards all TCP traffic arriving on the localhost interface at 127.0.0.1 through port 2222 to port 22 in the guest.

# It is possible to configure incoming NAT connections while the VM is running, see Section 8.13, “VBoxManage controlvm”.
# 显示虚拟机信息
vboxmanage showvminfo UServer01 
vboxmanage list natnets

VBoxManage controlvm UServer1 natpf2 "web-yarn,tcp,10.1.62.62,8088,192.168.56.201,8089"
# 添加NAT服务网络
vboxmanage natnetwork add --netname vboxnat --network "10.34.76.3/24" --enable
vboxmanage modifyvm cszubt23 --nic2 natnetwork --nat-network1 vboxnat
# Manual "6.3.1. Configuring port forwarding with NAT" says this: It works, only for stopped VMs.
# But for already-running VM the correct command is:
vboxmanage modifyvm --bridgeadapter2 enp2s0
# 克隆虚拟机
vboxmanage clonevm UServer2 --mode all UServer5 --basefolder /home/dc/4T/vm/ --register
# 控制已经运行的虚拟机：(暂停｜恢复｜重启｜关机｜休眠)
vboxmanage controlvm | pause|resume|reset|poweroff|savestate
# 上面例子中设置了两个网络接口，Nic1 是 NAT 模式， Nic2 是 Bridge 模式。这里 Nic2 绑定的网络接口名字是 enp0s25.  下面我们改成当前机器的网络接口名字

# bitcoin
cd $BITCOIN_HOME
nohup bitcoind &
bitcoin-cli help getwalletinfo
bitcoin-cli --getinfo
bitcoin-cli getwalletinfo
bitcoin-cli getblockcount
g++ --version
pkg-config --variable pc_path pkg-config

bitcoin-cli walletpassphrase $btcpass 60
bitcoin-cli sendtoaddress 1Mee3ctzBZ1LTx6HHJ99bev6rVKE1UXcDC 0.0001  "send some to address:Mee...DC" "DC" false
#### 选择默认编辑器
select-editor
常用编辑器
vi/vim 
nano
emacs


#### 远程仓库命令
列出可使用的 .gitignore 模板
```
curl -X GET --header 'Content-Type: application/json;charset=UTF-8' 'https://gitee.com/api/v5/gitignore/templates?access_token='${GITEE_TOKEN}
```
获取一个 .gitignore 模板
```
curl -X GET --header 'Content-Type: application/json;charset=UTF-8' 'https://gitee.com/api/v5/gitignore/templates/Gradle?access_token=bc04a015a3bdda7d708a1859d256d654'
```
获取仓库所有任务标签
```
curl -X GET --header 'Content-Type: application/json;charset=UTF-8' 'https://gitee.com/api/v5/repos/DC-Melo/aaattt/labels?access_token=bc04a015a3bdda7d708a1859d256d654'
```
列出仓库所有的tags
```
curl -X GET --header 'Content-Type: application/json;charset=UTF-8' 'https://gitee.com/api/v5/repos/DC-Melo/aaattt/tags?access_token=bc04a015a3bdda7d708a1859d256d654'
```
列出授权用户的所有仓库
```
curl -X GET --header 'Content-Type: application/json;charset=UTF-8' 'https://gitee.com/api/v5/user/repos?access_token=bc04a015a3bdda7d708a1859d256d654&sort=created&page=1&per_page=20'
```
