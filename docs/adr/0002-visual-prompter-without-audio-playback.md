# Visual prompter without audio playback

The application operates as a visual recitation guide and reader rather than an audio player. We decided against bundling or streaming recorded chant audio because devotees chant at their own personal vocal cadence and breath capacity; an external audio track imposes an unnatural pace and distracts the reciter. The pacing engine calculates natural Sanskrit metrical weights (`matras()`) and breath pauses at dandas solely to drive visual guidance.

## Considered Options

- **Bundled audio playback with synced timestamps**: Rejected because personal recitation requires vocal freedom, and audio playback turns active chanting into passive listening.
- **Pure static text without prompter**: Rejected because reciters frequently lose their place during rapid group chanting or when learning complex mantras.

## Consequences

- The app requires zero audio bandwidth, avoids audio decoding latency, and remains under 50 KB in total asset weight.
- Auto-advancing visual prompter acts as a flexible guide, easily adjusted via the speed control or paused at will.
