# TrackMeOrNot

TrackMeOrNot is a web browser that enables users to selectively share their online footprints with vendors (trackers) while shielding privacy sensitive browsing activities from trackers.

TrackMeOrNot is implemented on Chromium (version 45.0.2426.3).

A user of TrackMeOrNot can specify URL policies and category policies as her/his tracking preference.
All navigations in TrackMeOrNot start with an anonymous browsing context.
The machine learning model in TrackMeOrNot automatically determines the topic of the content that the user is visiting.
Based on the user's tracking preference, TrackMeOrNot can seamlessly switch to or stay in the expected browsing context (persistent or anonymous) to meet user's privacy need.

You can find more information about TrackMeOrNot in our [WWW 2016 research paper](http://www.cc.gatech.edu/~wmeng6/www16_trackmeornot.pdf). Please reference our paper when using TrackMeOrNot or our data set. The BibTeX format file is provided with the source code.

## Research Project Page

http://www.cc.gatech.edu/~wmeng6/trackmeornot.html

## Build Instructions

The steps to building TrackMeOrNot are similar to those to building Chromium. You only need to sync with the version of Chromium on which TrackMeOrNot was implemented and apply two GIT patches. The full instruction for building Chromium could be found [here](http://dev.chromium.org/developers/how-tos/get-the-code).

1. Set up the environment
You need to install the *depot_tools* package first. [Instructions](https://commondatastorage.googleapis.com/chrome-infra-docs/flat/depot_tools/docs/html/depot_tools_tutorial.html#_setting_up)
  ```
  mkdir chromium
  cd chromium
  ```

2. Fetch the Chromium source code
  ```
  fetch --nohooks chromium
  ```

3. Install dependencies
  1. On Linux:
    ```
    sudo ./build/install-build-deps.sh
    ```

  2. On Mac, please see the [instructions](https://chromium.googlesource.com/chromium/src/+/master/docs/mac_build_instructions.md).


4. Check out to the track-me-or-not branch
  ```
  cd src
  git checkout -b track-me-or-not 45.0.2426.3
```

5. Sync with version 45.0.2426.3
  ```
  gclient sync --with_branch_heads --jobs 16
  ```

6. Apply Chromium patch
  ```
  git apply ../../patches/trackmeornot_chromium.patch
  ```

7. Apply WebKit patch
  ```
  cd third_party/WebKit
  git apply ../../../../patches/trackmeornot_webkit.patch
  ```

8. Run post-sync hooks
  ```
  cd ../../
  gclient runhooks
  ```

9. Build Chromium
  ```
  ninja -C out/Release chrome
  ```

10. Run TrackMeOrNot
  1. On Linux
    ```
    TRACKING_CONTROL=1 out/Release/chrome
    ```

  2. On Mac OS X
    ```
    TRACKING_CONTROL=1 out/Release/Chromium.app/Contents/MacOS/Chromium
    ```

## Data Set

Please find details about the data set used in our WWW 2016 research paper in the research project page.

## Classification Model

We built 78 binary classifiers using the LinearSVC algorithm with our data set, which was labeled with [AlchemyAPI](http://www.alchemyapi.com).
The classifiers were used for evaluation in our paper.
Since we have not got the permission to release the model learnt from the data set, we do not include the classifiers in the source code release.
Instead, we provide a dummy classificaiton model implementation in our release.
The dummy classification model randomly selects one from the 78 classes as the prediction result.

You could build your own classifiers and integrate with TrackMeOrNot.
The implementation of the dummy classification model could be found in `chromium/src/third_party/WebKit/Source/core/tmon/`.

## Copyright Information

Copyright © 2016 Georgia Institute of Technology

### Additional Notes

Notice that some files in TrackMeOrNot may carry their own copyright notices.
In particular, TrackMeOrNot's code release contains modifications to source files from the Google Chromium project (https://www.chromium.org), which are distributed under their own original license.

## License

TrackMeOrNot is licensed under the [MIT License](http://www.opensource.org/licenses/mit-license.php).

Copyright (c) 2016 Georgia Institute of Technology

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Developers 

Wei Meng <wei@gatech.edu>

Byoungyoung Lee <blee@gatech.edu>

## Contact ##

Wei Meng <wei@gatech.edu>
