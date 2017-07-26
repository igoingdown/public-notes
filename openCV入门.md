## 安装

```sh
brew install opencv
```

## 设置环境变量

```bash
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:/usr/local/Cellar/opencv/2.4.13.2/lib/pkgconfig
```

## 编写cpp文件

* 示例cpp文件`test.cpp`
    ```cpp
    #include <cv.h>
    #include <cvaux.h>
    #include <highgui.h>

    using namespace std;
    using namespace cv;


    int main()
    {
      Mat image =
          imread("../caltech-lanes/test1/00a3ee0ae672e8a7bdf6dcb67b1ea74b.jpg");
      Mat I;
      cvtColor(image,I,CV_BGR2GRAY);
      Mat contours;
      Canny(I,contours,640,350);
      threshold(contours,contours,128,255,THRESH_BINARY);
      vector<Vec4i> lines(2, Vec4i(4, 1));
      for (int i = 0; i < 4; i++) {
        lines[0][i] = 0 * i;
        lines[1][i] = 20 * i;
      }
      auto it = lines.begin();
      while(it != lines.end())
      {
        Point pt1((*it)[0], (*it)[1]);
        Point pt2((*it)[2], (*it)[3]);
        line(image, pt1, pt2, Scalar(0,255,0), 2); //  线条宽度设置为2
        ++it;
      }
      namedWindow("Lines");
      imshow("Lines", image);
      waitKey();
      return 0;
    }

    ```

## 编译

```bash
g++ test.cpp -std=c++11 `pkg-config --cflags --libs opencv`  -o test

# mac上编译调用opencv库的cpp文件
```
