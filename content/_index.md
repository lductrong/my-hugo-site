+++
title = "Building a Serverless Text-to-Speech Application with Amazon Polly"
date = 2021
weight = 1
chapter = false
+++

# Building a Serverless Text-to-Speech Application with Amazon Polly

## Overview

Text-to-speech (TTS) conversion is a complex technology that requires in-depth research. It’s possible for some words to be read out without any meaning at all. Factors contributing to these errors in TTS applications include: **homographs (words that are spelled the same but pronounced differently), text normalization, phonetic conversion, proper names, slang, and abbreviations.**

Amazon Polly provides the ability to overcome these issues, allowing us to focus on building applications that utilize TTS technology without worrying about algorithmic problems.

## Introduction to Amazon Polly

Amazon Polly is Amazon Web Services' (AWS) text-to-speech (TTS) service. Polly uses deep learning technology to convert text into natural-sounding speech in multiple languages, making it easy to integrate into applications for creating high-quality audio content.

**Key Features of Amazon Polly:**

- **Natural-sounding Voices**: Polly supports dozens of realistic voices across more than 20 languages.
- **Quick Response Time**: The service provides real-time response times, suitable for applications like virtual assistants and chatbots.
- **Easy Integration**: Polly is easy to integrate via API.
- **SSML Support**: Polly supports Speech Synthesis Markup Language (SSML), allowing customization of voice with elements like intonation, speed, and pronunciation.
- **Cost-effective**: There are no additional charges for using converted audio.
- **Storage and Playback Capability**: Polly allows the storage and playback of audio files for use without a network connection.

## Workshop Objectives

The Text-to-Speech (TTS) application diversifies information accessibility, especially for **visually impaired individuals**. It enhances user experience in virtual assistant systems, chatbots, or serves entertainment activities like listening to books, newspapers, and stories.

In this workshop, you will be introduced to how to build a serverless Text-to-Speech application with Amazon Polly, and how to integrate Amazon Polly into your application to convert text into natural-sounding speech.
