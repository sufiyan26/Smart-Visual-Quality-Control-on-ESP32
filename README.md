# Smart Visual Quality Control on ESP32

**TinyML-ready production line inspector** running on ESP32 (Wokwi simulation). Processes 100-frame batches, extracts brightness/texture/edge features, runs `runDefectInference()`, classifies OK/DEFECT, and reports defect rates over serial. Architecture supports seamless TensorFlow Lite Micro integration.

## Core Concept & Objective

- **Goal:** Edge-based defect detection without cloud dependency.
- **Key Feature:** Pluggable inference function—current rule-based classifier can be swapped with TFLite Micro model without changing control logic.

## Technical Pipeline
Frame Loop (100 frames/run):
      Synthesize features: [brightness, texture_roughness, edge_density] ∈​
      Inference: runDefectInference(features) → probability ∈​
      Decision: probability > threshold → DEFECT
      Log: frameID, score, decision → Serial
      Summary: defect_count / 100 → defect_rate %

## Project Structure
├── firmware/
│ ├── qc_controller.cpp # Main loop + inference
│ ├── defect_model.h # Rule-based classifier (TFLite ready)
│ └── tflite_model.h # Pre-converted TensorFlow Lite C array
├── wokwi/
│ └── diagram.json # ESP32 production line simulation
└── logs/
└── sample_run.log # 100-frame diagnostics


## Example Serial Output
Frame 012: score=0.18 → OK
Frame 045: score=0.81 → DEFECT
Frame 067: score=0.65 → DEFECT
Frame 089: score=0.23 → OK
=== Run Complete: 14/100 DEFECTS (14.0%) ===


## Defect Detection Logic
float runDefectInference(float b, float t, float e) {
// Rule-based (TFLite compatible signature)
float score = 0.0;
if (b < 0.3) score += 0.4; // Too dark
if (t > 0.7) score += 0.3; // Too rough
if (e < 0.2) score += 0.3; // No edges
return score;
}


## Skills Demonstrated

ESP32 firmware • TinyML/TensorFlow Lite Micro integration • Real-time feature extraction • Wokwi simulation • Production QC algorithms • Serial diagnostics • Modular inference architecture






