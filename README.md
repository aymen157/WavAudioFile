## Overview
`WavAudioFile` is a C# Wav parser. can be used in game engines (eg. unity) to load audio at runtime. useful for faster iteration times.

## Features
- Parses WAV file headers and data.
- Supports PCM and WaveFormatExtensible formats.
- Converts audio data to floating-point format for easier processing.
- Integration with Unity for easy audio playback.

## Structure Definition

```csharp
public partial struct WavAudioFile
{
    public string ChunkID;
    public uint ChunkSize;
    public string Format;
    public string Subchunk1ID;
    public uint Subchunk1Size;
    public ushort AudioFormat;
    public string AudioFormatName => FormatCode(AudioFormat);
    public ushort NumChannels;
    public uint SampleRate;
    public uint ByteRate;
    public ushort BlockAlign;
    public ushort BitsPerSample;
    public string Subchunk2ID;
    public uint Subchunk2Size;
    public byte[] Data;
    public float[] DataAsFloat => getDataAsFloat();

    // Method to parse the WAV file from a stream
    public static WavAudioFile Parse(Stream stream);
    
    // Method to parse the WAV file from byte array
    public static WavAudioFile Parse(byte[] data);
    
    public override string ToString();
}
```

## Example usage
using System.IO;

```csharp
// Load WAV file from disk
using (var fileStream = File.OpenRead("path/to/your/file.wav"))
// or byte[] wavData = File.ReadAllBytes("path/to/your/file.wav");
{
    WavAudioFile wavFile = WavAudioFile.Parse(fileStream);
    Console.WriteLine(wavFile.ToString());
}
```
Unity:
```csharp
void FootstepSoundEmitter(string footstepSoundFile)
{
    var wav = WavAudioFile.Parse(System.IO.File.ReadAllBytes(footstepSoundFile));
    var clip = wav.ToAudioClip();
    AudioSource src = Camera.main.gameObject.AddComponent<AudioSource>();
    src.clip = clip;
    src.Play();
}
```

## Supported Formats
Currently only PCM. 8, 16, 24, 32 bits per sample

## Notes
Ensure that your WAV file is in the correct format (PCM) for proper parsing.
You can use ffmpeg to convert other audio formats to PCM WAV:
```ffmpeg -i input.mp3 -acodec pcm_s16le -ar 44100 -ac 2 output.wav```
