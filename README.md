# README

This repository includes the data and tools used in the paper _An Automatic Participant Detection Framework for Event Tracking on Twitter_.
This README.md file describes how you can use the repository to experiment with the data yourself.

## Configuration

You can use the configuration file at `config/conf.py` to control the logging level.
This logging level is used while detecting participants, and can be restricted to reduce verbosity.

## Data

This repository contains the data used in the evaluation of _An Automatic Participant Detection Framework for Event Tracking on Twitter_.
Due to [Twitter's Developer Policy](https://developer.twitter.com/en/developer-terms/policy), it is not possible for us to share the original tweets.
Instead, we are only allowed to share the tweet IDs.
The tweet IDs are stored in `data/ids`, separated by dataset.

An additional file—`data/ids/sample.json`—includes tweet IDs collected using Twitter's Streaming API.
This sample of tweets was used in the article to construct the TF-ICF table.
Instead of downloading it anew, you can use the `data/idf.json` file instead¸ which stores the TF-ICF scheme.

If you prefer to download a new sample and use it to construct the TF-ICF scheme, you can use the `idf` tool:

    ./tools/idf.py --file ~/DATA/evaluation/apd/data/sample.json \
    --output results/tfidf.json \
    --stem \
    --remove-unicode-entities \
    --normalize-words

To learn more about this tool and the options it accepts, use `./tools/idf.py --help`.

## Generating participants

To generate participants, use the `participants` tool available in the `tools` directory.
The baselines can be generated using the following snippets.
In all cases, the tools expect the file to contain tweets: one tweet per line.
Therefore before running the following, you need to download the tweets anew from the provided IDs.

To generate the NLTK baseline:

    ./tools/participants.py --file data/TOTLEI.json \
    --output results/TOTLEI.json \
    --extractor EntityExtractor

To generate the TwitterNER baseline:

    ./tools/participants.py --file data/TOTLEI.json \
    --output results/TOTLEI.json \
    --extractor TwitterNEREntityExtractor

To generate participants using NLTK and TwitterNER with the algorithm described in the article, you need to provide the TF-ICF scheme described above.
Once the TF-ICF scheme is ready, run the following to identify participants with this algorithm, using NLTK to extract named entities:

    ./tools/participants.py --file data/TOTLEI.json \
    --output results/TOTLEI.json \
    --model ELDParticipantDetector \
    --extractor EntityExtractor \
    --tfidf data/idf.json

To do the same thing, but extracting participants using TwitterNER, run the following instead:

    ./tools/participants.py --file data/TOTLEI.json \
    --output results/TOTLEI.json \
    --model ELDParticipantDetector \
    --extractor TwitterNEREntityExtractor \
    --tfidf data/idf.json

> Note: you might need to download additional data for TwitterNER: more details about this library are available in the [TwitterNER GitHub repository](https://github.com/napsternxg/TwitterNER).
> The downloaded files can be added to the `lib/apd/extractors/local/TwitterNER/data` directory in this repository.
