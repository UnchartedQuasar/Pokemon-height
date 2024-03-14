# -*- coding: utf-8 -*-
"""
Created on Thu Mar 14 13:05:22 2024

@author: Vijay 2.0
"""
import streamlit as st
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
import requests
import numpy as np



st.title('Pokemon with streamlit')


@st.cache_data

def get_details(poke_number):
    ''' Create an entry for our favourite pokemon '''
    try:
        url = f'https://pokeapi.co/api/v2/pokemon/{poke_number}/'
        response = requests.get(url)
        pokemon = response.json()
        return pokemon['name'], pokemon['height'], pokemon['weight'], len(pokemon['moves']), pokemon['sprites']['front_default'], pokemon['cries']['latest'], pokemon['types'][0]['type']['name']
    except:
        return 'Error', np.NAN, np.NAN, np.NAN, np.NAN, np.NAN, np.NAN


pokemon_number = st.slider('Pick a Pokemon!', min_value=1
                           , max_value = 1025)

name, height, weight, moves, sprites, cry, colour = get_details(pokemon_number)
height *= 10

height_data = pd.DataFrame({'pokemon': ['weedle', name.title(), 'victreebel'],
               'heights':[30, height, 170]})

colour_types = {'normal' :'gray',
                'fire': 'red',
                'water': 'dodgerblue',
                'electric': 'yellow',
                'grass': 'limegreen',
                'ice': 'lightblue',
                'fighting': 'sienna',
                'poison': 'mediumorchid',
                'ground': 'goldenrod',
                'flying': 'cornflowerblue',
                'psychic': 'hotpink',
                'bug': 'yellowgreen',
                'rock': 'darkkhaki',
                'ghost': 'slateblue',
                'dragon': 'mediumpurple',
                'dark': 'saddlebrown',
                'steel': 'darkgray',
                'fairy': 'violet'}


colours = ['yellowgreen', colour_types[colour], 'limegreen']

graph = sns.barplot(data = height_data,
                    x='pokemon',
                    y='heights',
                    palette = colours)

st.write(f'Name: {name.title()}')
st.write(f'Type: {colour.title()}')
st.write(f'height: {height}')
st.write(f'weight: {weight}')
st.write(f'moves: {moves}')
st.audio(cry, format='audio/ogg')

st.pyplot(graph.figure)


col1, col2, col3 = st.columns(3)

with col1:
   st.image('https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/13.png', use_column_width=True)

with col2:
   st.image(sprites, use_column_width=True)

with col3:
   st.image('https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/71.png', use_column_width=True)
