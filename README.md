// ---------------- ReadingScreen ----------------
import 'package:flutter/material.dart';
import 'package:speech_to_text/speech_to_text.dart' as stt;
import 'package:flutter_tts/flutter_tts.dart';
import 'dart:math';

class ReadingScreen extends StatefulWidget {
  const ReadingScreen({super.key});

  @override
  State<ReadingScreen> createState() => _ReadingScreenState();
}

class _ReadingScreenState extends State<ReadingScreen> {
  final stt.SpeechToText _speech = stt.SpeechToText();
  final FlutterTts _tts = FlutterTts();
  bool _isListening = false;
  String _spokenText = "";
  Map<String, int> _scores = {};

  final String passage =
      "English is an international language. It helps us to communicate with people across the world.";

  void _startListening() async {
    bool available = await _speech.initialize();
    if (available) {
      setState(() => _isListening = true);
      _speech.listen(onResult: (result) {
        setState(() {
          _spokenText = result.recognizedWords;
        });
      });
    }
  }

  void _stopListening() {
    _speech.stop();
    setState(() => _isListening = false);
  }

  void _speak() async {
    await _tts.speak(passage);
  }

  void _submitAnswer() {
    int fluency = Random().nextInt(20) + 80;
    int pronunciation = Random().nextInt(20) + 80;
    int completeness =
        (_spokenText.split(" ").length / passage.split(" ").length * 100)
            .clamp(0, 100)
            .toInt();
    int accuracy = Random().nextInt(20) + 80;

    setState(() {
      _scores = {
        "Fluency": fluency,
        "Pronunciation": pronunciation,
        "Completeness": completeness,
        "Accuracy": accuracy,
      };
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Read Aloud")),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text("Passage:", style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold)),
            const SizedBox(height: 10),
            Text(passage, style: const TextStyle(fontSize: 16)),
            const SizedBox(height: 20),
            Text("You said: $_spokenText", style: const TextStyle(color: Colors.grey)),

            const Spacer(),

            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                _buildButton(Icons.mic, _isListening ? "Stop" : "Record", () {
                  _isListening ? _stopListening() : _startListening();
                }),
                _buildButton(Icons.play_arrow, "Listen", _speak),
                _buildButton(Icons.check, "Submit", _submitAnswer),
              ],
            ),

            const SizedBox(height: 20),
            if (_scores.isNotEmpty) _buildResultCard(),
          ],
        ),
      ),
    );
  }

  Widget _buildButton(IconData icon, String text, VoidCallback onTap) {
    return Column(
      children: [
        InkWell(
          onTap: onTap,
          child: CircleAvatar(radius: 30, child: Icon(icon, size: 30)),
        ),
        const SizedBox(height: 8),
        Text(text),
      ],
    );
  }

  Widget _buildResultCard() {
    return Card(
      margin: const EdgeInsets.all(8),
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: _scores.entries
              .map((e) => Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,
                    children: [
                      Text(e.key, style: const TextStyle(fontSize: 16)),
                      Text("${e.value}%",
                          style: const TextStyle(fontWeight: FontWeight.bold))
                    ],
                  ))
              .toList(),
        ),
      ),
    );
  }
}
