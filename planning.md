# TakeMeter Planning Document

## 1. Community
My community is the Reddit Pokémon community (`r/pokemon`). This is a highly opinionated community that spans gameplay information, subjective takes, inquiries on strategies, and more. Given the varying quality of discourse, it is a great choice to be able to distinguish, especially for readers that are looking for a specific piece of information or simply want to share their opinions. 

## 2. Labels
**`subjective_take`** A post making a bold claim or critique based entirely on personal feeling, nostalgia, or aesthetic preference without verifiable evidence.
* Example 1: "Gen 1 has the best starters, you can't beat the classics that look like actual monsters."
* Example 2: "Yeah, a decent amount of people who've been around since before Gen 6 attribute the fact that Pokemon in general started going downhill once they transitioned from 2D to 3D. It's not really a hot take, more of a well-known take."

**`analysis`**
A post that makes an argument supported by specific, verifiable evidence—like citing base stats, quoting specific game text, or doing competitive math.
* Example 1: "Not sure if this is a hot take, but Ice should resist Water and Dragon. It shouldn't get a defense buff in Snow though. Aurora Veil exists and we don't want to make Ice Pokemon on the snow even bulkier when they can already freeze you to death with 100% Blizzard spam."
* Example 2: "HM's used to be important parts of puzzles and dungeons. None of those things exist in the recent games. I miss the obnoxious 'move these boulders in the right order' aspect of things. It gave players a reason to go back to areas they've already visited. Turning HM's into simple keys you carry removes the strategy that comes with dungeon diving and team building."

**`strategy_inquiry`**
A post actively soliciting help, team-building advice, or asking a direct question about how to overcome an in-game obstacle.
* Example 1: "I'm stuck on Cynthia's Garchomp in BDSP. My current team is Empoleon, Staraptor, Luxray, Rapidash, Roserade, and Lucario. Should I swap out Luxray for a dedicated Ice type like Weavile, or just try to overlevel my Empoleon so it can tank a hit?"
* Example 2: "Need help building a VGC Reg F team around Walking Wake. I know setting up Sun is mandatory, so I'm running Torkoal, but I keep getting completely walled by Flutter Mane. Any advice on specific EV spreads or good defensive pivots I should add?"

## 3. Hard Edge Cases
An expected edge case is a post that sits between a `subjective_take` and `analysis` because it contains an opinion but attempts to provide reasoning behind it without verifiable facts. 

**Example Post:**
> *"I'll go first, my hot take is Gen 9 has the best starter lineup out of all the gens. Skeledirge is bulky with a strong special attacking stat and a recovery move. Meowscarada is a fast physical glass cannon with a move that never misses and always crits. Quaquaval has a very strong physical attack stat and a signature move that boosts speed."*

**Decision Rule:** Given that it states "best" and gives subjective evidence such as "a strong special attacking stat" without quantified numbers, I will classify it as `subjective_take`. To be classified as `analysis`, the post must rely on verifiable evidence or quantified numbers (e.g., citing a Base 110 Special Attack stat). 

## 4. Data Collection Plan
I will collect 200 public posts from the `r/pokemon` subreddit. To avoid turning this data collection step into a longer coding project or writing a custom scraper, I will manually gather batches of text and use an LLM to format them directly into a CSV structure. As I collect, I will monitor the label distribution to ensure I am finding enough `analysis` and `strategy_inquiry` posts, and I will specifically hunt for more of those if `subjective_take` becomes too overrepresented (avoiding any single label crossing the 70% mark).

## 5. Evaluation Metrics
We need to calculate the precision PER class, not just lump everything together into overall accuracy, given that the classes won't be an even split due to the highly opinionated nature of the `r/pokemon` community. We also need to ensure the model is actively recalling information by finding matching posts and not just skipping the harder ones. These two aspects will be balanced using the F1-score (which goes from 0 to 1). A higher F1-score will show that the model is both trustworthy in its precision and thorough in finding posts that fit the specific labels.

## 6. Definition of Success
For this classifier to be genuinely useful, we want it to reliably find and categorize posts across all labels. Therefore, achieving a high F1-score on our minority classes (`analysis` and `strategy_inquiry`) is the ideal metric. However, having an exceptionally high F1-score (like 95%) would likely mean the model is not working correctly—it might be memorizing the training data or relying on an oversimplified taxonomy. Therefore, achieving an F1-score around 0.80 (80%) on those specific classes is the ideal target to prove the model successfully learned the discourse boundaries.

## 7. AI Tool Plan
* **Annotation Assistance:** I will use an LLM to pre-batch label my gathered examples based on my provided taxonomy and format them into my CSV structure. I will then manually review every single classification.
* **Label Stress-Testing:** Having the LLM pre-label the data will serve as a direct stress-test. If I agree with its classifications, it proves my labels and definitions are working correctly as intended. It will also allow me to analyze any early failures and tighten my label boundaries before committing to the full 200-post dataset.
* **Failure Analysis:** After fine-tuning my model, I will use an LLM to look at the list of wrong predictions to help identify patterns or blind spots in how the model learned the discourse.
