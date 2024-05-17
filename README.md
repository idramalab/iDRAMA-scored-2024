
![iDRAMA-Scored-2024 Header](https://huggingface.co/datasets/iDRAMALab/iDRAMA-scored-2024/resolve/main/idrama-scored-2024-banner-orig.png?download=true)

# Dataset Summary

`iDRAMA-Scored-2024` is a large-scale dataset containing approximately 57 million social media posts from web communities on social media platform, Scored.
Scored serves as an alternative to Reddit, hosting banned fringe communities, for example, c/TheDonald, a prominent right-wing community, and c/GreatAwakening, a conspiratorial community.
This dataset contains 57M posts from over 950 communities collected over four years, and includes sentence embeddings for all posts.

- **Scored platform:** [Scored](https://scored.co)
- **Link to paper:** [Here](https://arxiv.org/abs/2405.10233)
- **License:** [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en)
  
  | Repo-links | Purpose |
  |:------|:--------------|
  | [Zenodo](https://zenodo.org/records/10516043) | From Zenodo, researchers can download `lite` version of this dataset, which includes only 57M posts from Scored (not the sentence embeddings). |
  | [Github](https://github.com/idramalab/iDRAMA-scored-2024) | The main repository of this dataset, where we provide code-snippets to get started with this dataset. |
  | [Huggingface](https://hf.co/datasets/iDRAMALab/iDRAMA-scored-2024) | On Huggingface, we provide complete dataset with senetence embeddings. |

# Getting Started

- **Loading dataset from Huggingface:** [huggingface_loader.ipynb](https://github.com/idramalab/iDRAMA-scored-2024/blob/401fcd9814dfa06bbb9066cb3d21840c6bc4d682/huggingface_loader.ipynb)
- **Loading dataset from Zenodo:** [zenodo_loader.ipynb](https://github.com/idramalab/iDRAMA-scored-2024/blob/401fcd9814dfa06bbb9066cb3d21840c6bc4d682/zenodo_loader.ipynb)
- **Explore dataset & attributes:** [explore_dataset.ipynb](https://github.com/idramalab/iDRAMA-scored-2024/blob/401fcd9814dfa06bbb9066cb3d21840c6bc4d682/explore_dataset.ipynb)

# Dataset Info

Dataset is organized by yealry-comments and submissions -- comments-2020, comments-2021, comments-2022, comments-2023, submissions-2020-t0-2023.

<table style="width:50%">
  <tr>
    <th style="text-align:left">Config</th>
    <th style="text-align:left">Data-points</th>
  </tr>
  <tr>
    <td>comments-2020</td>
    <td>12,774,203</td>
  </tr>
  <tr>
    <td>comments-2021</td>
    <td>16,097,941</td>
  </tr>
  <tr>
    <td>comments-2022</td>
    <td>12,730,301</td>
  </tr>
  <tr>
    <td>comments-2023</td>
    <td>8,919,159</td>
  </tr>
  <tr>
    <td>submissions-2020-to-2023</td>
    <td>6,293,980</td>
  </tr>
</table>

<details>
  <summary>  <b> Top-15 communities in our dataset with total number of posts are shown as following: </b> </summary>
  
| Community | Number of posts |
|:------------|:---------------|
| c/TheDonald | 41,745,699 |
| c/GreatAwakening | 6,161,369 |
| c/IP2Always | 3,154,741 |
| c/ConsumeProduct | 2,263,060 |
| c/KotakuInAction2 | 747,215 |
| c/Conspiracies | 539,164 |
| c/Funny | 371,081 |
| c/NoNewNormal | 322,300 |
| c/OmegaCanada | 249,316 |
| c/Gaming | 181,469 |
| c/MGTOW | 175,853 |
| c/Christianity | 124,866 |
| c/Shithole | 98,720 |
| c/WSBets | 66,358 |
| c/AskWin | 39,308 |

</details>

<details>
  <summary>  <b> Submission data fields are as following: </b> </summary>
  
```yaml
- `uuid`: Unique identifier associated with each sub- mission (uuid).
- `created`: UTC timestamp of the submission posted to Scored platform.
- `date`: Date of the submission, converted from UTC timestamp while data curation.
- `author`: User of the submission. (Note -- We hash the userames for ethical considerations.)
- `community`: Name of the community in which the submission is posted to.
- `title`: Title of the submission.
- `raw_content`: Body of the submission.
- `embedding`: Generated embedding by combining "title" and "raw_content," with 768 dimensional vector with fp32-bit.

- `link`: URL if the submission is a link.
- `type`: Indicates whether the submission is text or a link.
- `domain`: Base domain if the submission is a link.
- `tweet_id`: Associated tweet id if the submission is a Twitter link.
- `video_link`: Associated video link if the submission is a video.

- `score`: Metric about the score of sample submission.
- `score_up`: Metric about the up-votes casted to sample submission.
- `score_down`: Metric about the down-votes casted to sample submission.

- `is_moderator`: Whether the submission is created by moderator or not.
- `is_nsfw`: True, if the submission is flagged not safe for work.
- `is_admin`: Boolean flag about whether the submission is posted by admin.
- `is_image`: Boolean flag if the submission is image type of media.
- `is_video`: Boolean flag if the submission is type of video.
- `is_twitter`: Boolean flag if the submission is a twitter (now, named as X) link.
- `is_deleted`: Whether the submission was deleted as a moderation measure or not. If yes, the "title" and "raw_content" could be empty string.

- `post_flair_text` & `post_flair_class`: Similar to Reddit submission flairs, which is a way to tag a submission with a certain keywords.
```
</details>

<details>
  <summary>  <b> Comments data fields are as following: </b> </summary>
  
```yaml
- `uuid`
- `date`
- `author`
- `community`
- `raw_content`
- `created`
- `embedding`
- `score`
- `score_up`
- `score_down`
- `is_moderator`
- `is_deleted`
```
</details>

> Read more about the fields and methodology from the [paper](https://arxiv.org/abs/2405.10233).

### Dataset fields Nullability:

- If field (column) doesn't have a value, the fields are left with an empty value.
  - For instance, in the case of post deletion as a moderation measure, `title` of submission can have no value.
  - We do not explicitly mark value as "Null" for any of the column in our dataset except `embedding` column.
- Only, embedding column contains explicit "Null" value.
  
For eliminating empty records using `pandas`, the code looks like below:

```python
# Load dataset for `comments-2020` config
dataset = load_dataset("iDRAMALab/iDRAMA-scored-2024", name="comments-2020")
pd_df = dataset["train"].to_pandas()

# Remove all empty records based on empty `title` column
pd_df = pdf_df[pd_df.title != ""]

# Remove all records which do not have `author` information
pd_df = pdf_df[pd_df.author != ""]

# Remove all records which do not have generated embeddings
pd_df = pdf_df[~pd_df.embedding.isna()]

```

# Version

- **Maintenance Status:** Active
- **Version Details:**
  - *Current Version:* v1.0.0
  - *First Release:* 05/16/2024
  - *Last Update:* 05/16/2024

# Authorship
This dataset is published at "AAAI ICWSM 2024 (INTERNATIONAL AAAI CONFERENCE ON WEB AND SOCIAL MEDIA)" hosted at Buffalo, NY, USA.

- **Academic Organization:** [iDRAMA Lab](https://idrama.science/people/)
- **Affiliation:** Binghamton University, Boston University, University of California Riverside

# Licensing
This dataset is available for free to use under terms of the non-commercial license [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en).

# Citation

```bibtex
@misc{patel2024idramascored2024,
      title={iDRAMA-Scored-2024: A Dataset of the Scored Social Media Platform from 2020 to 2023}, 
      author={Jay Patel and Pujan Paudel and Emiliano De Cristofaro and Gianluca Stringhini and Jeremy Blackburn},
      year={2024},
      eprint={2405.10233},
      archivePrefix={arXiv},
      primaryClass={cs.SI}
}
```
