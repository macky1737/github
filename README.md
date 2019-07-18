# github
初めて使うGithubで何を作ろうかと思ったところ、ネット記事に“iPhoneで正確に脈を測って記録できる「Instant Heart Rate」"という内容のものが上がっていたので、似たような物をjupyterLabで作ってみた。「Instant Heart Rate」は、iPhoneのカメラ機能を使って心拍数を測るユニークなアプリだ。カメラに指を当て、LEDの光で脈を読み取ることにより、目分量に比べて正確な脈を測定できるらしい。  
なので今回作ったコードにも、カメラに指を押し付けてもらうことで血液の赤色の輝度を取得し平均化、その値を毎フレームごとにグラフにプロットする（これで脈拍を測れる予定）内容となっている。
# 実行結果

![](https://raw.github.com/wiki/macky1737/github/images/a.gif)  

![](https://raw.github.com/wiki/macky1737/github/images/f.gif)  
gifアニメーションにしていたが、スマホで撮ったため（指でカメラを押さえている絵が欲しかった）やむなくMOVファイルになった。
# コード
```
pip install opencv-python
import cv2
import matplotlib.pyplot as plt
import numpy as np
from collections import deque
from IPython.display import display, clear_output
capture = cv2.VideoCapture(0)
queue = deque([0]*10)
i=-1
# Plot
plt.ion()
fig = plt.figure()
while(True):
    ret, frame = capture.read()
    frame1 = frame[:,:,0]
    h,w= frame1.shape
    mean = 0
    for y in range(h):
        for x in range(w):
            mean += frame1[y, x]
    mean /= h * w
    queue.popleft()
    queue.append(mean)
    i += 1
    # Plot
    clear_output(wait = True)
    plt.cla()
    plt.plot(range(i-9,i+1),queue)
    plt.grid()
    plt.ylim(20,25)
    display(fig) 
    print(mean)
    cv2.imshow('frame',frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
capture.release()
cv2.destroyAllWindows()
```
# 使い方
使い方は、実行すると内カメラが起動するので人差し指をカメラに押し付けてもらうと、グラフが血液の輝度をプロットして値が下に表示される。  
使ったのはopencv.
# 参考資料
参考にしたサイトはhttps://qiita.com/iton/items/89dfdf3e1672219d91e4
 でこのサイトからはqueueの使い方とグラフの更新の仕方を参考にさせてもらった。コードとしては  
 capture = cv2.VideoCapture(0)  
 while(True):
    ret, frame = capture.read()  
 capture.release()
cv2.destroyAllWindows()の部分である  
https://weblabo.oscasierra.net/python/opencv-videocapture-camera.html　
このサイトからはopencvでカメラから動画を取得する方法を参考にさせてもらった。コードとしては  
queue = deque([0]*10)  
plt.ion()  
fig = plt.figure()  
queue.popleft()  
    queue.append(mean)  
clear_output(wait = True)  
    plt.cla()  
    plt.plot(range(i-9,i+1),queue)  
    plt.grid()  
    plt.ylim(20,25)  
    display(fig) の部分である
