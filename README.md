Accessible Image Understanding Assistant with Spoken Output

Multimodal AI prototype: image captioning, VQA, and uncertainty-aware clarifying questions, plus spoken output and a Confidence-Checked Action Planner that resolves ambiguous instructions instead of guessing. Built as independent research proof-of-work for MEXT scholarship applications.

What it does

This project explores multimodal AI systems that don't just answer confidently — they recognize when they're uncertain and ask for clarification instead of guessing wrong. It combines:

Image captioning — generates natural-language descriptions of images.
Visual Question Answering (VQA) — answers open-ended questions about an image.
Uncertainty-aware clarifying questions — checks its own answer consistency across rephrased questions, and asks for clarification instead of guessing when it detects genuine ambiguity.
Spoken output — converts responses to audio, aimed at accessibility for visually impaired users.
How it works

Built using Qwen2-VL-2B-Instruct (Hugging Face Transformers) for image understanding and generation, and Google Text-to-Speech (gTTS) for audio output, all run in Google Colab. The uncertainty-detection method asks the same question twice with slightly different phrasing and compares the two answers for consistency — disagreement signals ambiguity worth flagging rather than guessing through.

Results

Tested end-to-end across 20 real images, including several deliberately ambiguous cases (multiple similar objects, unclear subjects). The pipeline successfully generated captions, answered visual questions, correctly flagged several genuinely ambiguous images, and produced spoken audio output for each result.

Honest limitation found: the original uncertainty-detection heuristic (comparing the first 30 characters of two answers) was too brittle and produced false positives on clearly answerable images. Switching to a word-overlap-based comparison (Jaccard-style similarity) reduced false positives but didn't eliminate them entirely — roughly 15 of 20 images still triggered the clarifying-question flag in one run, suggesting the threshold may still be tuned too sensitively. This is documented as an open, honest finding rather than something papered over.

Extension: Confidence-Checked Action Planner

This extension applies the same uncertainty-detection idea to robot-style instructions instead of image questions. Given a small simulated room with known objects, the system interprets natural-language instructions (e.g., "bring me the thing I read") and decides which object is meant — asking for clarification instead of guessing when the instruction is genuinely ambiguous (e.g., "go to it").

Results: Correctly identified objects from indirect descriptions (e.g., "the round one" → ball, "the thing I read" → book, "the thing that rings" → phone). Found a further honest limitation: the same instruction ("go to it") was flagged as ambiguous in one run but not in another, showing that consistency-checking between two independently-generated AI responses isn't perfectly reliable — the underlying model has some inherent randomness each time it generates text, so agreement between two guesses doesn't always guarantee the instruction was actually clear.

See Confidence-Checked Action Planner.ipynb for the full code, robot_results.csv for results, and the accompanying PNG images for visual output.

Connection to current multimodal AI research

This project connects to active research directions in vision-language models: image captioning and VQA (the core pipeline), uncertainty and reliability in multimodal systems (the clarifying-question mechanism), spoken multimodal output (the voice extension), and ambiguity resolution in instruction-following (the Confidence-Checked Action Planner extension) — an area with direct relevance to robotics and human-AI interaction research.

Tools used

Python, Hugging Face Transformers, Qwen2-VL-2B-Instruct, PyTorch, Google Text-to-Speech (gTTS), Matplotlib, Pandas, Google Colab.

Files
Main notebook: image captioning + VQA + uncertainty detection + voice output pipeline
results.csv — results from the main 20-image test run
Confidence-Checked Action Planner.ipynb — the robot instruction extension
robot_results.csv — results from the robot instruction test run
Sample audio and image outputs
About

Built independently while preparing graduate research applications (MEXT Scholarship, Japan) in multimodal AI. Feedback and discussion welcome.
