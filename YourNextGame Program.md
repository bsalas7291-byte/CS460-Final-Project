YourNextGame Program // using weighted ranking algorithm

PlayerPreferences:
    budget
    prefGenre
    prefPlaytime
    prefCoop

GameStat:
    title
    genre
    cost
    playtime
    coop
    hoursPlayed
    completed

FUNCTION getStats(library):
// gets player stats from their game library
// gets most played genre, playtime for genres, coop playtime

  genreHours
  singleCount
  coopCount
  completedGenre

  For each game in libary
    If game.genre not in genreHours
        genreHours[game.genre] = 0

    genreHours[game.genre] = genreHours[game.genre] + game.hoursPlayed

    If game.coop == true
        coopCount =+ 1
    Else 
        singleCount =+ 1

    If game.completed == true
        If game.genre not in completedGenres
            completedGenres[game.genre] = 0
        completedGenre[game.genre] =+ 1

    mostPlayedGenre = genre with most hours played
    mostCompletedGenre = genre completed the most

    If singleCount > coopCount
        prefMode = singleplayer
    Else
        prefMode = coop

    return mostPlayedGenre,, mostCompletedGenre, prefMode

FUNCTION score(game, preference, stats):
// gives each unowned game a score based on
// how much they match preferences and stats
// determines what game is the current best match for the player

    score = 0

    If game.cost <= preferences.budget
        score =+ 20
    Else
        score =- 20
    
    If game.genre == preferneces.prefGenre
        score =+ 20
    
    If game.genre == playerStats.mostPlayedGenre
        score =+ 15

    If game.genre == playerStats.mostCompletedGenre
        score =+ 15
    
    return score

FUNCTION getNextGame(library, games, preferences):
// finds games that match player preferences and stats and
// checks if they are unowned by the player
// recommends the best match by score

    playerStats = getStats(library)

    bestGame
    highestScore

    For each game in games
        If game.owned == false
            currentScore = score(game, preferneces, playerStats)

            If currentScore > highestScore
                highestScore = currentScore
                bestGame = game
    
    return bestGame