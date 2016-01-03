NT Histogram
==========

Provides functions to create histogram from Bitmap images.

- Works on UWP (Projects for Windows10)
* For WindowsPhone 8/8.1, See [NtImageLib](https://github.com/naotaco/NtImageLib)
- Suppoorts to analyze two bitmap classes:
 * WriteableBitmap (Windows.UI.Xaml.Media.Imaging)
 * CanvasBitmap ([Microsoft.Graphics.Canvas](https://github.com/Microsoft/Win2D))

## Two projects

Use NtHistogram to analyze WriteableBitmap on UWP projects.

If you are using [Win2D](https://github.com/Microsoft/Win2D), you should use NtHistogramWin2d to analyze from CanvasBitmap.

## Usage

A class, HistogramCreator supports to show level of each colors.

```cs
HistogramCreator histogramCreator;

private void SomethingInitialize()
{
    // Initialize instance with resolution setting
    histogramCreator = new HistogramCreator(HistogramCreator.HistogramResolution.Resolution_128);
    histogramCreator.OnHistogramCreated += histogramCreator_OnHistogramCreated;

    // If you don't need to analyze all pixels, specify value bigger than 0 here to skip pixels. It helps to reduce CPU power.
    // To calculate all pixels, set 0 here.
    HistogramCreator.PixelSkipRate = 10;
}

public void StartToAnalyzeImage()
{
    // input image as WriteableBitmap or CanvasBitmap
    await histogramCreator.CreateHistogram(ImageSource);
}

void histogramCreator_OnHistogramCreated(int[] Count_R, int[] Count_G, int[] Count_B)
{
    // Do something with values. e.g.) show line charts
    // Length of these arrays will be same, and it will be the specified resolution(64, 128 or 256).
    // Each values in the arrays represents numbers of levels.

    // in case specified resolution is 128,
    int r0 = Count_R[0]; // a count of pixels that it's value is 0 or 1 in Red channel
    int r63 = Count_R[63]; // a count of pixels that has value 126 or 127.

    // in case specified resolution is 256,
    int r0 = Count_R[0]; // a count of pixels that it's value is 0
    int r63 = Count_R[63]; // a count of pixels that has value 63
}

```