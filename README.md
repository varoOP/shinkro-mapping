# shinkro-mapping

- Community mapping for use with [shinkro](https://github.com/varoOP/shinkro).
- Pull requests are welcome and appreciated!

## Description of `tvdb-mal-master.yaml`:

- This is the file that needs to be manually edited. The rest are generated automagically, `tvdb-mal.yaml` is used by shinkro.
- It is a mapping between anime titles as listed on MyAnimeList (MAL) and their corresponding entries on TheTVDB, a TV show database.

### Each entry in the mapping contains:

1. **`malid`**: The unique identifier for an anime on MyAnimeList.

2. **`title`**: The title of the anime.

3. **`type`**: The type of the media, which can be 'tv', 'ova', etc.

4. **`tvdbid`**: The unique identifier for an anime or TV show on TheTVDB. A value of `0` indicates that the series is currently unmapped to any TVDB entry.

5. **`tvdbseason`**: Represents the season of the series on TVDB. A value of `0` usually indicates a specials season.

6. **`start`**: The starting episode number from where the mapping begins. A value of `0` is common and indicates that the mapping begins from the very first episode.

7. **`useMapping`**: Enables or disables the use of animeMapping. Boolean value `true` or `false`.

8. **`animeMapping`**: A list of mappings between TVDB seasons and MAL episodes. Each mapping contains:
   - **`tvdbseason`**: The season number on TVDB.
   - **`start`**: The starting episode number on MAL for the mapped TVDB season.
   - **`mappingType`** (optional): Specifies the type of mapping. Can be `"explicit"` or `"range"` (default is `"range"`). See below for details.
   - **`explicitEpisodes`** (optional): A map of TVDB episode numbers to MAL episode numbers. Used when `mappingType` is `"explicit"`. Format: `{tvdbEpisode: malEpisode}`. Example: `{7: 6, 8: 11, 9: 16}` means TVDB E07 → MAL E06, TVDB E08 → MAL E11, etc.
   - **`skipMalEpisodes`** (optional): An array of MAL episode numbers to skip when using range mapping. Used when `mappingType` is `"range"` or omitted. Example: `[6, 11, 16]` will skip MAL episodes 6, 11, and 16 when counting.

### Mapping Types:

#### Range Mapping (Default)
- Used for linear episode mappings where TVDB episodes map sequentially to MAL episodes.
- Can optionally skip specific MAL episodes using `skipMalEpisodes`.
- Example: TVDB S03E01-E23 maps to MAL E01-E05, E07-E10, E12-E15, E17-E26 (skipping E06, E11, E16).

```yaml
animeMapping:
  - tvdbseason: 3
    start: 1
    mappingType: "range"  # Optional, defaults to "range"
    skipMalEpisodes:
      - 6
      - 11
      - 16
```

#### Explicit Mapping
- Used when episodes don't follow a linear pattern and need individual mappings.
- Each TVDB episode is explicitly mapped to a specific MAL episode.
- Useful for special episodes, summaries, or non-contiguous mappings.
- Example: TVDB S00E07, E08, E09 map to MAL E06, E11, E16 respectively.

```yaml
animeMapping:
  - tvdbseason: 0
    start: 0
    mappingType: "explicit"
    explicitEpisodes:
      7: 6   # TVDB S00E07 → MAL E06
      8: 11  # TVDB S00E08 → MAL E11
      9: 16  # TVDB S00E09 → MAL E16
```

### Common Mapping Scenarios:

#### Scenario 1: Simple Single Season Mapping
For most entries, you just need to set the `tvdbid` and `tvdbseason`. Some anime which have mid season ends in TVDB but are different anime in MAL, also need the `start` entry to be set. For example, some anime will be like this on MAL: ThisThat Season 2 Part 1, ThisThat Season 2 Part 2. These will just be ThisThat Season 2 in TVDB. So both anime must be set to the same with `tvdbid` and the same `tvdbseason` but with a different `start` value. If season ThisThat Season 2 Part 2 starts on episode 13, then `start` will be you be set to 13. Check the mapping for DanMachi Season 4 for a proper example.

#### Scenario 2: Multiple Seasons (One Entry on MAL)
This is only required for anime that have multiple seasons on TVDB but just one season on Myanimelist. The prime example of this is One Piece.

```yaml
useMapping: true
animeMapping:
  - tvdbseason: 10
    start: 196
  - tvdbseason: 12
    start: 326
  - tvdbseason: 15
    start: 517
```

#### Scenario 3: Special Episodes Mapped to Season 0
When some MAL episodes (like summaries) are mapped to TVDB Season 0, use explicit mapping:

```yaml
useMapping: true
animeMapping:
  - tvdbseason: 0
    start: 0
    mappingType: "explicit"
    explicitEpisodes:
      7: 6   # TVDB S00E07 → MAL E06 (summary)
      8: 11  # TVDB S00E08 → MAL E11 (summary)
      9: 16  # TVDB S00E09 → MAL E16 (summary)
  - tvdbseason: 3
    start: 1
    skipMalEpisodes:   # Skip the summary episodes
      - 6
      - 11
      - 16
```

#### Scenario 4: Range Mapping with Skipped Episodes
When a TVDB season maps to MAL episodes but some MAL episodes are skipped (e.g., they're mapped elsewhere):

```yaml
useMapping: true
animeMapping:
  - tvdbseason: 3
    start: 1
    skipMalEpisodes:
      - 6
      - 11
      - 16
```

This maps TVDB S03E01-E23 to MAL episodes, skipping MAL E06, E11, and E16. The algorithm counts non-skipped episodes: S03E06 → MAL E07 (6th non-skipped), S03E23 → MAL E26 (23rd non-skipped).

### How to Contribute:

1. Fork the repository.

2. Identify a series from MyAnimeList that is not mapped to TheTVDB.

3. Fill in the `tvdbid` with the appropriate identifier from TheTVDB.

4. Adjust the `tvdbseason` and `start` values as necessary.

5. If specific season-to-episode mappings are required, set `useMapping` to `true` and provide the necessary mappings in the `animeMapping` list.

6. For complex mappings:
   - Use `mappingType: "explicit"` with `explicitEpisodes` for non-linear episode mappings
   - Use `mappingType: "range"` (or omit it) with `skipMalEpisodes` when episodes need to be skipped
   - You can combine both in the same entry for different seasons

7. If you're unsure about the mapping, you can leave the values as `0`, `[]` or `false` as appropriate, and someone will review the PR before merging.

8. Create a pull request.

## Description of `tmdb-mal-master.yaml`:

- This mapping only contains anime movies which aren't already mapped in the database of shinkro i.e `shinkro.db`.
- This is the file that needs to be manually edited. The rest are generated automagically, `tmdb-mal.yaml` is used by shinkro.
- It is mapping between anime titles, as recognized by the main title, and their corresponding entries on MyAnimeList (MAL) and The Movie Database (TMDb).

### Each entry in this mapping contains:

1. **`mainTitle`**: The primary title of the anime movie.

2. **`tmdbid`**: The unique identifier for an anime movie on The Movie Database (TMDB). A value of `0` indicates that the anime movie is currently unmapped to any TMDb entry.

3. **`malid`**: The unique identifier for an anime movie on MyAnimeList (MAL).

### How to Contribute:

1. Fork the repository.

2. Identify a movie from the list that is not mapped to TMDB.

3. Find the correct entry for movie on TMDB and note its unique identifier.

4. Update the `tmdbid` field with the appropriate identifier from TMDB.

5. Create a pull request.