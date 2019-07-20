# github 
今回作ったコードは、カメラに指を押し付けてもらうことで血液の赤色の輝度を取得し平均化、その値を毎フレームごとにグラフにプロットする（これで脈拍を測れる予定）という内容。
# 実行結果

![](https://raw.github.com/wiki/macky1737/github/images/a.gif)  
![](https://raw.github.com/wiki/macky1737/github/images/g.MOV)
gifアニメーションにしていたが、スマホで撮ったため（指でカメラを押さえている絵が欲しかった）やむなくMOVファイルのリンクのみ貼っておく
https://raw.github.com/wiki/macky1737/github/images/g.MOV  (トリミングして3.11MB)

# 使い方
使い方は、実行すると内カメラが起動するので人差し指をカメラに押し付けてもらうと、グラフが血液の輝度をプロットして値が下に表示される。  
使ったのはopencv 4.1.0.,matplolib 3.0.3  
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
