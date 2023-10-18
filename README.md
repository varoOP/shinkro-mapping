# shinkro-mapping

- Community mapping for use with [shinkro](https://github.com/varoOP/shinkro).
- Pull requests are welcome and appreciated!

## Description of `tvdb-mal-master.yaml`:

- This is the file that needs to be manually edited. The rest are generated automagically, `tvdb-mal.yaml` is used by shinkro.
- It is a mapping between anime titles as listed on MyAnimeList (MAL) and their corresponding entries on TheTVDB, a TV show database.
- Each entry in the mapping contains:

1. `malid`: The unique identifier for an anime on MyAnimeList.
2. `title`: The title of the anime.
3. `type`: The type of the media, which can be 'tv', 'ova', etc.
4. `tvdbid`: The unique identifier for an anime or TV show on TheTVDB. A value of `0` indicates that the series is currently unmapped to any TVDB entry.
5. `tvdbseason`: Represents the season of the series on TVDB. A value of `0` usually indicates a specials season.
6. `start`: The starting episode number from where the mapping begins. A value of `0` is common and indicates that the mapping begins from the very first episode.
7. `useMapping`: Enables or disables the use of animeMapping. Boolean value `true` or `false`.
8. `animeMapping`: A list of mappings between TVDB seasons and MAL episodes. Each mapping contains:
   - `tvdbseason`: The season number on TVDB.
   - `start`: The starting episode number on MAL for the mapped TVDB season.
   - This is only required for anime that have multiple seasons on TVDB but just one season on Myanimelist. The prime example of this is One Piece.

- For most entries, you just need to set the `tvdbid` and `tvdbseason`. Some anime which have mid season ends in TVDB but are different anime in MAL, also need the `start` entry to be set. For example, some anime will be like this on MAL: ThisThat Season 2 Part 1, ThisThat Season 2 Part 2. These will just be ThisThat Season 2 in TVDB. So both anime must be set to the same with `tvdbid` and the same `tvdbseason` but with a different `start` value. If season ThisThat Season 2 Part 2 starts on episode 13, then `start` will be you be set to 13. Check the mapping for DanMachi Season 4 for a proper example.

#### How to Contribute:

1. Fork the repository.
2. Identify a series from MyAnimeList that is not mapped to TheTVDB.
3. Fill in the `tvdbid` with the appropriate identifier from TheTVDB.
4. Adjust the `tvdbseason` and `start` values as necessary.
5. If specific season-to-episode mappings are required, set `useMapping` to `true` and provide the necessary mappings in the `animeMapping` list.
6. If you're unsure about the mapping, you can leave the values as `0`, `[]` or `false` as appropriate, and someone will review the PR before merging.
7. Create a pull request.

## Description of `tmdb-mal-master.yaml`:

- This mapping only contains anime movies which aren't already mapped in the database of shinkro i.e `shinkro.db`.
- This is the file that needs to be manually edited. The rest are generated automagically, `tmdb-mal.yaml` is used by shinkro.
- It is mapping between anime titles, as recognized by the main title, and their corresponding entries on MyAnimeList (MAL) and The Movie Database (TMDb).
- Each entry in this mapping contains:

1. `mainTitle`: The primary title of the anime movie.
2. `tmdbid`: The unique identifier for an anime movie on The Movie Database (TMDB). A value of `0` indicates that the anime movie is currently unmapped to any TMDb entry.
3. `malid`: The unique identifier for an anime movie on MyAnimeList (MAL).

#### How to Contribute:

1. Fork the repository.
2. Identify a movie from the list that is not mapped to TMDB.
3. Find the correct entry for movie on TMDB and note its unique identifier.
4. Update the `tmdbid` field with the appropriate identifier from TMDB.
5. Create a pull request.
