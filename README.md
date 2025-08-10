# 1. Business Problem 
On our messaging platform, length group conversations have become difficult to follow when everyone is busy, and important details get lost in the noise. 
* Messaging platforms generate too much content. Users spend an average of 3-5 minutes catching up on missed conversations, and 20% report missing important information. As a result, engagement drops for users with 10+ active chats.
* To increase customer satisfaction and user retention, we developed this automated dialogue summarization, which will save time, boost engagement. It positions our platform as a leader in smart communication and creates opportunities for premium features. 
## Key objectives and success criteria
* Technical metrics: 
  * ROUGE-1: >0.40, capture most key words from human summaries
  * ROUGE-2: > 0.15), preserve enough key phrase connections 
  * ROUGE-L: > 0.35, keep the main structure/order intact
* Business metrics:
  * Reduce catch-up time by 50% (down to ~1.5-2.5 minutes)
  * Lower “missed information info” complaints by 20%
  * Improve  engagement for heavy-chat users.
# 2. Technical Approach 
* Data preparation and preprocessing decisions: 
  * Dataset: SAMSum – 16k+ human-annotated messenger-style dialogues.
  * Preprocessing: 
      * Filtered out invalid dialogues
      * Used AutoTokenizer from the t5-small model. 
      * max input length = 384 tokens, max target length = 96 tokens (Chosen after EDA to balance coverage of dialogue content with avoidance of truncation.)
  * Model architecture: 
      * Base model: t5-small, a transformer from Hugging Face specifically designed as an encoder-decoder model for sequence-to-sequence tasks like summarization. 
      * Encoder: Captures context and structure of dialogue
      * Decoder: Generates fluent summaries
* Training approach and evaluation / optimization techniques:
  * Used ROUGE quantitative benchmarking and human side-by-side comparison for qualitative assessment
  * Ran a random search for decoder parameters for the best configurate: (num_beams=2, length_penalty=1.3, min_length=12) 
  * Evaluation framed as:
    * ROUGE-1: “Does it say the right things?”
    * ROUGE-2: “Does it connect them well?”
    * ROUGE-L: “Does it keep the story straight?”
  * Qualitative: Human inspection
# 3. Results
  * ROUGE
    * ROUGE-1: 0.4553, captures most key words from human summaries
    * ROUGE-2: 0.2060, preserves enough key phrase connections 
    * ROUGE-L: 0.3682, keeps the main structure/order intact
* Observations / What affects model performance:
    * The model handles structured event changes well but struggles with extracting emotional subtext or multi-threaded topics in very long chats.
    * Length of dialogue: because of our settings, if the dialogue exceeds 384 tokens, the model will miss information beyond that setting. 
    * Number of speakers: More speakers introduce more complexity, and the model might be confused by roles and ownership, attributing information to the wrong person or missing that detail as a whole. 
    * Improvement ideas:
      * As long dialogues drop performance, it would help to break them into chunks before summarizing
      * Add speaker tags or fine-tune with more examples to handle multi-speaker dialogues better
# 4. Business Impact and Conclusion
* Business value:
  * Shorter catch-up time → higher daily active usage.
  * Reduced frustration → lower churn.
  * Differentiation → stronger competitive positioning.
  * Potential for premium summarization features (e.g. per-topic highlights, searchable summaries, etc.)
* Achievements: 
  * Built an efficient, scalable summarization model
  * Hit key ROUGE performance goals
* Limitations and Recommendations: 
  * Some drop in performance for long, multi-threaded dialogues.
  * Recommend:
    * Users summarize dialogues under 384 tokens, warning pop-up if too long
    * Beta deployment to measure real-world engagement lift

