# Video Player

This video player component is designed in such a way to track users watch progress exactly.This means if a user fast forwards the video or rewinds and watches again that wont doesnt effect his progress.

The true progress is only counted if he truly watches the entire video and there are no shortcuts to skip the video with 100% progress

How the logic works:
 The entire video is divided into segments. The number of segments are based on the duration of the video.

 When ever the user watches the a segment it is marked as completed and to acheive 100% progress he needs to watch all the segments.

 If the user fast forwards and watches future segments then the progress increases but he will not acheive a 100% as he skipped some segments.

 If he rewinds since they are already marked watched so the logic doesn't count that as progress.
