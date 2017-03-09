# HuyIT
#include<iostream>
#include<opencv2\imgproc\imgproc.hpp>
#include<opencv2\highgui\highgui.hpp>
#include<opencv2\core\utility.hpp>

using namespace cv;
using namespace std;
void nhapanh(const char* path) {
	Mat img;
	img = imread(path, IMREAD_GRAYSCALE);
	if (img.empty()) return;
	int rate[256] = { 0 };
	for (int i = 0; i < img.rows; i++) {
		for (int j = 0; j < img.cols; j++) {
			rate[(int)img.at<uchar>(i, j)]++;
		}
	}
	for (int i = 0; i < 256; i++) {
		if (rate[i] != 0)
		{
			cout << "[" << i << "]: " << rate[i] << " ";
		}
	}
	system("pause");
}

void anhmau(string path) {    //const char* path
	Mat img = imread(path, IMREAD_COLOR);
	if (img.empty()) return;
	int rateR[256] = { 0 };
	int rateG[256] = { 0 };
	int rateB[256] = { 0 };
	for (int i = 0; i < img.rows; i++) {
		for (int j = 0; j < img.cols; j++) {
			rateR[(int) img.at<Vec3b>(i, j)[2]]++;
		}
	}
	for (int i = 0; i < 256; i++)
	{
		std::cout << i << " " << rateR[i] << " ";
	}
	system("pause");
}

void TangDoSang(const char*url)
{
	Mat img;
	img = imread(url, IMREAD_COLOR);

	Mat imgOut = Mat::zeros(img.rows, img.cols, CV_8UC3);
	if (img.empty() == true)
	{
		std::cout << "Cannot load image" << std::endl;
		return;
	}
	for (int i = 0; i < imgOut.rows; i++)
	{
		for (int j = 0; j < imgOut.cols;j++)
		{
			cout << (int)img.at<Vec3b>(i, j)[0] << " " << (int)img.at<Vec3b>(i, j)[1] << " " << (int)img.at<Vec3b>(i, j)[2];
		}
	}
	namedWindow("Display standard", WINDOW_AUTOSIZE);
	imshow("Display standard", img);
	namedWindow("Display", WINDOW_AUTOSIZE);
	imshow("Display", imgOut);
	waitKey(0);
}
void splitChannelsInColorImage(const char*fname)
{
	Mat img;
	img = imread(fname, IMREAD_COLOR); ///// Read the file
	if (img.empty() == true)
	{
		std::cout << "Cannot load image" << std::endl;
		return;
	}
	Mat redChannel = Mat::zeros(img.rows, img.cols, CV_8UC3);
	Mat greenChannel = Mat::zeros(img.rows, img.cols, CV_8UC3);
	Mat blueChannel = Mat::zeros(img.rows, img.cols, CV_8UC3);
	for (int i = 0; i < img.rows; i++)
	{
		for (int j = 0; j < img.cols; j++)
		{
			redChannel.at<Vec3b>(i, j)[2] = img.at<Vec3b>(i, j)[2];
			greenChannel.at<Vec3b>(i, j)[1] = img.at<Vec3b>(i, j)[1];
			blueChannel.at<Vec3b>(i, j)[0] = img.at<Vec3b>(i, j)[0];
		}
	}
	namedWindow("Display window", WINDOW_AUTOSIZE); // Create a window for display.
	imshow("Display window", img);                // Show our image inside it.
	namedWindow("Display Red", WINDOW_AUTOSIZE); // Create a window for display.
	imshow("Display Red", redChannel);
	namedWindow("Display Green", WINDOW_AUTOSIZE); // Create a window for display.
	imshow("Display Green", greenChannel);
	namedWindow("Display Blue", WINDOW_AUTOSIZE); // Create a window for display.
	imshow("Display Blue", blueChannel);
	waitKey(0); // Wait for a keystroke in the window
}


void LocTrungBinh(const char*url,int TanSoLoc)
{
	Mat img;
	img = imread(url,IMREAD_GRAYSCALE);
	Mat imgOutput;
	imgOutput = imread(url, IMREAD_GRAYSCALE);
	for (int m = 0; m < TanSoLoc; m++)
	{
		for (int i = 1; i < imgOutput.rows-1; i++)
		{
			for (int j = 1; j < imgOutput.cols-1; j++)
			{
				try
				{
					
					imgOutput.at<uchar>(i, j) = (imgOutput.at<uchar>(i + 1, j) + imgOutput.at<uchar>(i + 1, j + 1) +
						imgOutput.at<uchar>(i, j + 1) + imgOutput.at<uchar>(i - 1, j + 1) + imgOutput.at<uchar>(i - 1, j)
						+ imgOutput.at<uchar>(i - 1, j - 1) + imgOutput.at<uchar>(i, j - 1) + imgOutput.at<uchar>(i + 1, j - 1) +
						imgOutput.at<uchar>(i, j)) / 9;
				}
				catch (const std::exception&)
				{
					cout << "error at location "<<i<<" "<<j<<" ";
				}
			}
		}
	}
	namedWindow("Display Window Standard", WINDOW_AUTOSIZE);
	imshow("Display Window Standard", img);
	namedWindow("Display Window Output", WINDOW_AUTOSIZE);
	imshow("Display Window Output", imgOutput);
	waitKey(0);
}

void LocChanel(Mat imgOutput,int Chanel)
{
	for (int i = 1; i < imgOutput.rows - 1; i++)
	{
		for (int j = 1; j < imgOutput.cols - 1; j++)
		{

			imgOutput.at<Vec3b>(i, j)[Chanel] = (imgOutput.at<Vec3b>(i + 1, j)[Chanel] + imgOutput.at<Vec3b>(i + 1, j + 1)[Chanel] +
				imgOutput.at<Vec3b>(i, j + 1)[Chanel] + imgOutput.at<Vec3b>(i - 1, j + 1)[Chanel] + imgOutput.at<Vec3b>(i - 1, j)[Chanel]
				+ imgOutput.at<Vec3b>(i - 1, j - 1)[Chanel] + imgOutput.at<Vec3b>(i, j - 1)[Chanel] + imgOutput.at<Vec3b>(i + 1, j - 1)[Chanel] +
				imgOutput.at<Vec3b>(i, j)[Chanel]) / 9;

		}
	}
}
void LocTrungBinhColor(const char* url,int TanSoLoc)
{
	Mat img;
	img = imread(url,IMREAD_COLOR);
	Mat imgOutput;
	imgOutput = imread(url, IMREAD_COLOR);
	for (int t = 0; t < TanSoLoc; t++)
	{
		LocChanel(imgOutput, 0);
		LocChanel(imgOutput, 1);
		LocChanel(imgOutput, 2);
	}

	namedWindow("Display Window Change", WINDOW_AUTOSIZE);
	imshow("Display Window Change", imgOutput);
	namedWindow("Display Window Standard",WINDOW_AUTOSIZE);
	imshow("Display Window Standard", img);

	waitKey(0);
}

int MediumValue(int Array[],int size)
{
	int value = 0;
	for (int i = 0; i < size; i++)
	{
		for (int j = i+1; j < size; j++)
		{
			if (Array[j] < Array[i])
			{
				int temp = Array[j];
				Array[j] = Array[i];
				Array[i] = temp;
			}
		}
	}
	value = Array[5];
	return value;
}

void LocTrungViAnhXam(const char* url, int TanSoLoc)
{
	Mat img; img = imread(url, IMREAD_GRAYSCALE);
	Mat imgOutput;imgOutput = imread(url, IMREAD_GRAYSCALE);
	int Array[15];
	for (int t = 0; t < TanSoLoc; t++)
	{
		for (int i = 1; i < imgOutput.rows-1; i++)
		{
			for (int j = 1; j < imgOutput.cols - 1; j++)
			{
				Array[0] = imgOutput.at<uchar>(i, j);
				Array[1] = imgOutput.at<uchar>(i + 1, j);
				Array[3] = imgOutput.at<uchar>(i + 1, j + 1);
				Array[4] = imgOutput.at<uchar>(i - 1, j + 1);
				Array[5] = imgOutput.at<uchar>(i - 1, j);
				Array[6] = imgOutput.at<uchar>(i - 1, j - 1);
				Array[7] = imgOutput.at<uchar>(i, j - 1);
				Array[8] = imgOutput.at<uchar>(i + 1, j - 1);
				imgOutput.at<uchar>(i, j) = MediumValue(Array, 9);
			}
		}					
	}
	namedWindow("Display", WINDOW_AUTOSIZE);
	imshow("Display", img);
	namedWindow("Display Window", WINDOW_AUTOSIZE);
	imshow("Display Window", imgOutput);

	waitKey(0);
}

int MediumValueChannel(Mat imgOutput,int channel,int i,int j)
{
	int value = 0;
	int Array[15];
	try
	{
		Array[0] = (int)imgOutput.at<Vec3b>(i, j)[channel];
		Array[1] = (int)imgOutput.at<Vec3b>(i + 1, j)[channel];
		Array[3] = (int)imgOutput.at<Vec3b>(i + 1, j + 1)[channel];
		Array[4] = (int)imgOutput.at<Vec3b>(i - 1, j + 1)[channel];
		Array[5] = (int)imgOutput.at<Vec3b>(i - 1, j)[channel];
		Array[6] = (int)imgOutput.at<Vec3b>(i - 1, j - 1)[channel];
		Array[7] = (int)imgOutput.at<Vec3b>(i, j - 1)[channel];
		Array[8] = (int)imgOutput.at<Vec3b>(i + 1, j - 1)[channel];
	}
	catch (const std::exception&)
	{
		cout << "Loi"<<endl;
	}
	value = MediumValue(Array,9);
	return value;
}

void LocTrungViAnhMau(const char* url, int TanSoLoc)
{
	Mat img; img = imread(url, IMREAD_COLOR);
	Mat imgOutput; imgOutput = imread(url, IMREAD_COLOR);
	int Array[15];
	for (int t = 0; t < TanSoLoc; t++)
	{
		for (int i = 1; i < imgOutput.rows - 1; i++)
		{
			for (int j = 1; j < imgOutput.cols - 1; j++)
			{
				
				imgOutput.at<Vec3b>(i, j)[0] = MediumValueChannel(imgOutput, 0, i, j);
				imgOutput.at<Vec3b>(i, j)[1] = MediumValueChannel(imgOutput, 1, i, j);
				imgOutput.at<Vec3b>(i, j)[2] = MediumValueChannel(imgOutput, 2, i, j);
				
				/*
				cout << (int)imgOutput.at<Vec3b>(i, j)[0] <<" ";
				cout << (int)imgOutput.at<Vec3b>(i, j)[1] << " ";
				cout << (int)imgOutput.at<Vec3b>(i, j)[2] << " ";*/
			}
		}
	}
	namedWindow("Display", WINDOW_AUTOSIZE);
	imshow("Display", img);
	namedWindow("Display Window", WINDOW_AUTOSIZE);
	imshow("Display Window", imgOutput);

	waitKey(0);
}

void PhanNguong(const char* url, int Nguong)
{
	Mat img,imgOutput;
	img = imread(url, IMREAD_GRAYSCALE);
	imgOutput = imread(url, IMREAD_GRAYSCALE);

	for (int i = 0; i < imgOutput.rows; i++)
	{
		for (int j = 0; j < imgOutput.cols; j++)
		{
			if (imgOutput.at<uchar>(i, j) > Nguong)
			{
				imgOutput.at<uchar>(i, j) = 0;
			}
			else
			{
				imgOutput.at<uchar>(i, j) = 255;
			}
		}
	}

	namedWindow("Display", WINDOW_AUTOSIZE);
	imshow("Display", img);
	namedWindow("Display Window", WINDOW_AUTOSIZE);
	imshow("Display Window", imgOutput);

	waitKey(0);
}

void CheckImageSame(const char* url1, const char* url2)
{
	Mat img1, img2;
	img1 = imread(url1, IMREAD_GRAYSCALE);
	img2 = imread(url1, IMREAD_GRAYSCALE);
	namedWindow("Display", WINDOW_AUTOSIZE);
	imshow("Display", img1);
	namedWindow("Display Window", WINDOW_AUTOSIZE);
	imshow("Display Window", img2);

	waitKey(0);
}
int main()
{
	//nhapanh("heart.png");
	//string s = "gas.jpg";
	//splitChannelsInColorImage("gas.jpg");
	//anhmau(s);
	//LocTrungBinh("frame.jpg",2);
	//LocTrungBinhColor("frame.jpg",2);
	//LocTrungViAnhXam("gas.jpg",2);
	//LocTrungViAnhMau("frame.jpg",1);
	//PhanNguong("gas.jpg", 155);
	//CheckImageSame("gas.jpg", "gas.jpg");
}
