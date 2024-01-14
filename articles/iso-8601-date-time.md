---
title: "ISO 8601 とは"
emoji: "📅"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["iso"]
published: true
---

## 一言で
ISO 8601 とは、日時を「YYYYMMDDThhmmss+|-hhmmss」または「YYYY-MM-DDThh:mm:ss+|-hh:mm:ssZ」と表すための国際規格です。



## 概要
この規格は、様々な国や文化によって異なる日付、時刻の表現を統一することを目的として定められています。

日付ははグレゴリオ暦、時刻は24時間表記で表現されます。



### 日付
日付表記のシンボルとして、「YYYY」「MM」「DD」が使用されています。

#### YYYY
西暦 (グレゴリオ暦)4桁で表します。

#### MM
01から12までの月を2桁で表します。

###### DD
01から28、30、31などの、その月の日を2桁で表します。

### 時刻
時刻表記のシンボルとして「hh」「mm」「ss」があります。

#### hh
00から24までの時間を表します。

2023-12-31T24:00:00 は 2024-01-01T00:00:00 と同義です。

#### mm
00から59までの分を表します。

#### ss
00から59までの秒を表します。

閏秒の場合、00から60までをとります。



# 参考
- [ISO - ISO 8601 — Date and time format](https://www.iso.org/iso-8601-date-and-time-format.html)
- [ISO 8601-1:2019 - Date and time — Representations for information interchange — Part 1: Basic rules](https://www.iso.org/standard/70907.html)
- [ISO 8601-2:2019 - Date and time — Representations for information interchange — Part 2: Extensions](https://www.iso.org/standard/70908.html)
- [日時のフォーマット（ISO 8601） - Qiita](https://qiita.com/kidatti/items/272eb962b5e6025fc51e)
- [ISO8601 とは？ - Zenn](https://zenn.dev/yass97/articles/2b5dcd5499ab07)
- [ISO 8601: Date and Time API の基礎知識 - Zenn](https://zenn.dev/khasunuma/articles/introduction-to-iso8601)
