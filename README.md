

# Esports-Data-to-Text

![Image text](https://github.com/ArnoZWang/esports-data-to-text/blob/main/examples/preview.jpg)

Scripts and related materials for [Commentary Generation from Data Records of Multiplayer Strategy Esports Game](https://arxiv.org/abs/2212.10935) ([Zihan Wang](https://www.tkl.iis.u-tokyo.ac.jp/~zwang/), Naoki Yoshinaga; NAACL SRW 2024).


# TL;DR

In this work, we set up a task of generating esports commentaries from structured data records of multiplayer strategy games; we built large-scale datasets, designed a set of evaluation criteria, and evaluated strong LLM-based baselines to reveal remaining challenges.


# Datasets

The first important contribution of this work is that we have constructed large-scale data-to-text datasets for one of the most popular esports games, [League of Legends](https://www.leagueoflegends.com/) (LoL).

The statistics of our esports data-to-text datasets is as follow. The table also includes the information of common datasets for similar tasks to show that our datasets are comparable in size to these data-to-text datasets.

|                       | **LoL19 (core)** | **LoL19-21 (extended)** | [LoL-V2T](https://ieeexplore.ieee.org/document/9522986) Video2Text | [RotoWire](https://aclanthology.org/D17-1239/) Basketball | [GameKnot](https://aclanthology.org/P18-1154/) Chess |
|-----------------------|:------------:|:-------------------:|:------------------:|:-------------------:|:--------------:|
| # games (matches)     |          220 |                 650 |                157 |               4,853 |         11,578 |
| # examples            |        3,490 |              10,590 |              9,723 |               4,853 |        298,008 |
| Avg. length of input  |       540.47 |              541.10 |        N/A (video) |              628.00 |          25.73 |
| Avg. length of output |       374.68 |              373.89 |               15.4 |              337.10 |          20.55 |

To protect data copyright and privacy, we will only provide the scripts used for collecting and processing the data, rather than the well-processed data itself. See this [example](https://github.com/ArnoZWang/esports-data-to-text/blob/main/examples/collect-data.ipynb).

## Example Input (Original)

> *… , {"type": "WARD_PLACED", "timestamp": 905433, "wardType": "YELLOWTRINKET", "creatorId": 6}, {"type": "WARD_KILL", "timestamp": 908742, "wardType": "YELLOWTRINKET", "killerId": 1}, {"type": "WARD_PLACED", "timestamp": 908775, "wardType": "CONTROLWARD", "creatorId": 5}, …*

## Example Input (Linearized)

> *… WARDPLACED|type 905433|timestamp YELLOWTRINKET|wardType 6|creatorId WARDKILL|type 908742|timestamp YELLOWTRINKET|wardType 1|killerId WARDPLACED|type 908775|timestamp CONTROLWARD|wardType 5|creatorId …*

## Example Output

> (linebreaks omitted)
> 
> *… just to stay even in a map state g2 can get exclusive vision on an area then suddenly the Nautilus veigar will have a lot of zone control but so behind in map control it's more about quick wards …*

## Example Output (ChatGPT Refinement)

> (work in progress, for experimental use only, not included in this work)
> 
> *… just to stay even in a map state. G2 can get exclusive vision on an area, then suddenly the Nautilus and Veigar will have a lot of zone control, but so behind in map control, it's more about quick wards …*

## Related Resources

Further information can be found at [Riot Developer](https://developer.riotgames.com/) and [LoL Fandom Wiki](https://lol.fandom.com/wiki/2019_Season_World_Championship) (World Championship details and links to contest videos). It is also highly recommended to access the Discord Channel "[Riot Games Third Party Developer Community](https://discordbotlist.com/servers/riotgamesdevrel)", where there are experienced developers who are interested in dealing with LoL-related data (LoL, TFT, LoR, Val, etc.).


# Evaluation Metrics

In this work, we use reference-based metrics for the other data-to-text tasks, and **perform human evaluation** based on the characteristics of esports.

## Reference-based Metrics (Automatic)

Following the existing data-to-text tasks, we adopt the following metrics for automatic evaluation.

 - [sacreBLEU](https://github.com/mjpost/sacreBLEU)
 - [normalized Damerau-Levenshtein distance](https://github.com/life4/textdistance) (text distance)
 - [ROUGE-L](https://github.com/pltrdy/rouge)
 - [BERTScore](https://pypi.org/project/bert-score)
 - [BARTScore](https://github.com/neulab/BARTScore) ([example of usage](https://github.com/ArnoZWang/esports-data-to-text/blob/main/examples/bart-score-test.ipynb))

> There are various implementations of these metrics; the links provided above are merely suggestions.

## Task-specific Metrics (Human Scoring)

Because it is difficult to estimate the strategic depth, we gather human scores using criteria tailored for esports commentaries. Human scoring aims to judge whether the output commentary **contains strategically relevant content** such as players' intentions and teams' arrangements.

The criteria of human scoring is detailed in the following table.

| **Strategic depth**                                                                                                                                                   | **score** |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------:|
| Based on the criteria for obtaining a score of 4, the strategic considerations are inspiring, providing insights to help learn from the skillful players and teams    |         5 |
| Based on the criteria for obtaining a score of 3, the strategic considerations are sufficient and closely related to the game moment described by the structured data |         4 |
| Based on explaining the facts, the commentary also reflects several strategic considerations, such as the player’s intention and the team’s arrangement               |         3 |
| The commentary only reflects the core event of the game moment described by the structured data, without providing any strategic consideration                        |         2 |
| The commentary reflects no facts or only a few facts of the game moment described by the structured data                                                              |         1 |


## Citation

Big welcome to star this repo and cite our work using the following BibTeX:

```bibtex
@inproceedings{wang2024commentary,
  title={Commentary Generation from Data Records of Multiplayer Strategy Esports Game},
  author={Wang, Zihan and Yoshinaga, Naoki},
  booktitle={Proceedings of the 2024 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 4: Student Research Workshop)},
  pages={263--271},
  year={2024}
}
```
