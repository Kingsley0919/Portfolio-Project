# =============================================================================
# Project1
# =============================================================================



import pandas as pd
import json
from os import chdir, getcwd

# Get the current working directory
wd = getcwd()

# Change the current working directory to the specified path where the datasets are located
chdir(r"C:\Users\User\Desktop\Master Degree\Semester 2\MECG11503 PROGRAMMING FOR DATA ANALYTICS\Assignment 2")

# Loading the datasets into pandas DataFrames
directors_df = pd.read_csv('directors.csv')
movies_df = pd.read_csv('movies.csv')

# Display the first few rows of each dataset for inspection
print("First few rows of the Directors dataset:")
print(directors_df.head())
print("\nFirst few rows of the Movies dataset:")
print(movies_df.head())

# Inspecting the structure and basic information of the two datasets to understand their schema and data types
print("\nBasic information about the Directors dataset:")
directors_df.info()
print("\nBasic information about the Movies dataset:")
movies_df.info()

# Checking for missing values in each dataset to identify if any preprocessing is needed
missing_values_directors = directors_df.isnull().sum()
missing_values_movies = movies_df.isnull().sum()
print("\nMissing values in the Directors dataset:")
print(missing_values_directors)
print("\nMissing values in the Movies dataset:")
print(missing_values_movies)

# Identifying duplicate records in each dataset to ensure data integrity before merging
duplicate_records_directors = directors_df.duplicated().sum()
duplicate_records_movies = movies_df.duplicated().sum()
print("\nNumber of duplicate records in the Directors dataset:")
print(duplicate_records_directors)
print("\nNumber of duplicate records in the Movies dataset:")
print(duplicate_records_movies)

# Filling missing values for 'overview' and 'tagline' columns with "Not Available"
# This is because these are text columns, and using "Not Available" avoids removing valuable data
movies_df['overview'].fillna('Not Available', inplace=True)
movies_df['tagline'].fillna('Not Available', inplace=True)

# Convert 'release_date' column to datetime format
movies_df['release_date'] = pd.to_datetime(movies_df['release_date'])

# Merging the movies and directors datasets on the director's ID to consolidate for data science modelling
# This uses the 'director_id' from movies_df and 'id' from directors_df
merged_df = pd.merge(movies_df, directors_df, left_on='director_id', right_on='id', suffixes=('_movie', '_director'))

# Dropping the redundant 'id_director' column post-merge to clean up the dataset
merged_df.drop('id_director', axis=1, inplace=True)

# Specifying the path to save the merged dataset
output_file_path = 'merged_movies_directors.csv'

# Saving the cleaned and merged dataset to a new CSV file, omitting the index for a cleaner file
merged_df.to_csv(output_file_path, index=False)



# =============================================================================
# Project2
# =============================================================================



import pandas as pd
import json
from os import chdir, getcwd

# Get the current working directory
wd = getcwd()

# Change the current working directory to the specified path where the datasets are located
chdir(r"C:\Users\User\Desktop\Master Degree\Semester 2\MECG11503 PROGRAMMING FOR DATA ANALYTICS\Assignment 2\dataset")

# Define a function to load a JSON file into a Python dictionary
def load_json(file_path):
    with open(file_path, 'r') as file:
        data = json.load(file)
    return data  # Returns a dictionary object

# Load various JSON datasets into Python dictionaries
museum_categories_USonly = load_json('museum_categories_USonly.json')
museum_categories_world = load_json('museum_categories_world.json')
review_content_USonly = load_json('review_content_USonly.json')
review_content_world = load_json('review_content_world.json')
review_quote_USonly = load_json('review_quote_USonly.json')
review_quote_world = load_json('review_quote_world.json')
tag_clouds_USonly = load_json('tag_clouds_USonly.json')
tag_clouds_world = load_json('tag_clouds_world.json')
traveler_rating_USonly = load_json('traveler_rating_USonly.json')
traveler_rating_world = load_json('traveler_rating_world.json')
traveler_type_USonly = load_json('traveler_type_USonly.json')
traveler_type_world = load_json('traveler_type_world.json')

# Load the Excel file into python csv
tripadvisor_merged = pd.read_csv('tripadvisor_merged.csv')

# Function to merge two dictionaries containing lists of categories and count the occurrences of each category
def merge_dicts_and_count_categories(dict1, dict2):
    # Combine values (lists of categories) from both dictionaries
    combined_values = list(dict1.values()) + list(dict2.values())
    # Flatten the list of lists
    all_categories = [item for sublist in combined_values for item in sublist]
    
    # Count occurrences of each category
    category_counts = {}
    for category in all_categories:
        if category in category_counts:
            category_counts[category] += 1
        else:
            category_counts[category] = 1
    
    # Convert the counts dictionary to a DataFrame
    df = pd.DataFrame(list(category_counts.items()), columns=['Category', 'TotalOccurrences'])
    return df

# Merge museum categories from US-only and worldwide datasets, then count occurrences
museum_categories_merge_df = merge_dicts_and_count_categories(museum_categories_USonly, museum_categories_world)
tag_clouds_merge_df= merge_dicts_and_count_categories(tag_clouds_USonly, tag_clouds_world)

# Compare sets of categories and tag clouds between the CSV dataset and merged JSON data, identifying missing values
entity_set_1_museum_catogories = tripadvisor_merged.iloc[0, 17:100]
entity_set_2_museum_catogories = museum_categories_merge_df.iloc[:,0].sort_values()

# Count the unique categories and tag clouds in the merged datasets and identify differences
entity_set_1_tag_clouds = tripadvisor_merged.iloc[0, 100:200]
entity_set_2_tag_clouds = tag_clouds_merge_df.sort_values(by='TotalOccurrences', ascending=False).iloc[:116, 0]

#Finding out the missing value in the excel as compare to the json file in tag_clouds
entity_set_1_traveler_type = tripadvisor_merged.iloc[:,200:206]
entity_set_2_tag_clouds = tag_clouds_merge_df.sort_values(by='TotalOccurrences', ascending=False).iloc[:116, 0]

# Count the length of each dictionary
length_USonly = len(traveler_type_USonly)
length_world = len(traveler_type_world)
print(f"Length of traveler_type_USonly dictionary: {length_USonly}")
print(f"Length of traveler_type_world dictionary: {length_world}")

# Since the length of the total traveler_type is different from the encoded column in the excel
#, we try to see what are the common key in these two datasets
traveler_type_common_keys = set(traveler_type_USonly.keys()) & set(traveler_type_world.keys())
length_common_keys = len(traveler_type_common_keys)
print(f"Length of traveler_type_common_keys dictionary: {length_common_keys}")


# Function to check for and handle duplicate keys between two dictionaries
def merge_dictionaries(dict1, dict2):
    """
    Merges two dictionaries and prints details about any duplicates with different values.
    If a key exists in both dictionaries with different values, the conflicting key from
    the second dictionary is renamed, and both versions are kept.
    
    Parameters:
        dict1 (dict): Please input USonly as the first dictionary to merge
        dict2 (dict): Please input world as the second dictionary whcich potentially same key with different value.
    
    Returns:
        tuple: The merged dictionary and a list of duplicates as (original_key, original_value, duplicate_key, duplicate_value).
    """
    merged_dict = {}
    duplicates = []

    # Add all items from the first dictionary
    merged_dict.update(dict1)

    # Iterate over items in the second dictionary
    for key, value in dict2.items():
        if key in merged_dict and merged_dict[key] != value:
            original_value = merged_dict[key]
            new_key = f"{key}_duplicate"
            counter = 1
            # Ensure the new key is unique
            while new_key in merged_dict:
                counter += 1
                new_key = f"{key}_duplicate_{counter}"
            merged_dict[new_key] = value
            duplicates.append((key, original_value, new_key, value))

            # Print details about the duplicate
            print(f"Duplicate Key with inconsistent value in US only dictionary: {key}, First Value: {original_value}")
            print(f"Duplicate Key with inconsistent value in world dictionary: {new_key}, Second Value: {value}")
            print("---")  # Separator for clarity
        elif key not in merged_dict:
            merged_dict[key] = value

    return merged_dict, duplicates

# Merge dictionaries and identify duplicates for traveler type and rating datasets
traveler_type_merged_dict, traveler_type_duplicates = merge_dictionaries(traveler_type_USonly, traveler_type_world)
traveler_rating_merged_dict, traveler_rating_duplicates = merge_dictionaries(traveler_rating_USonly, traveler_rating_world)

# Extract relevant variables

# Convert the dictionaries to pandas DataFrames with the specified structure
df_traveler_type_USonly = pd.DataFrame(list(traveler_type_USonly.values()), index=traveler_type_USonly.keys()).reset_index()
df_traveler_type_world = pd.DataFrame(list(traveler_type_world.values()), index=traveler_type_world.keys()).reset_index()

# Rename the 'index' column to 'MuseumName' and other columns as specified
traveller_type = ['MuseumName', "Families_Count", "Couples_Count", "Solo_Count", "Business_Count", "Friends_Count"]
df_traveler_type_USonly.columns = traveller_type
df_traveler_type_world.columns = traveller_type
traveler_type_common_keys = set(traveler_type_USonly.keys()) & set(traveler_type_world.keys())

# Remove duplicate key in the world dataframe to eliminate inconsistency
df_traveler_type_world = df_traveler_type_world[~df_traveler_type_world['MuseumName'].isin(traveler_type_common_keys )]

# Union both of the dataframe for comprehensive view of data
df_union = pd.concat([df_traveler_type_USonly, df_traveler_type_world], ignore_index=True)

#Create a subset of data by extracting data from merge excel file
tripadvisor_subset = tripadvisor_merged[['Latitude', 'Langtitude', 'PreciseRating', 'MuseumName']].copy()

# Perform a inner join operation based on 'MuseumName' to extract out the data for project 3
data_extracted = pd.merge(tripadvisor_subset, df_union, on='MuseumName', how='inner')
cols_to_convert = traveller_type[1:]
data_extracted[cols_to_convert] = data_extracted[cols_to_convert].apply(pd.to_numeric, errors='coerce')



# =============================================================================
# Project3
# =============================================================================



import dash
from dash import dcc, html
from dash.dependencies import Input, Output
import plotly.graph_objects as go
import plotly.express as px
import pandas as pd
import json
from os import chdir, getcwd

# Get the current working directory
wd = getcwd()

# Change the current working directory to the specified path where the datasets are located
chdir(r"C:\Users\User\Desktop\Master Degree\Semester 2\MECG11503 PROGRAMMING FOR DATA ANALYTICS\Assignment 2\dataset")

# Adding a custom hover text column for the map visualization
data_extracted['hover_text'] ='<br>Museum Name: '+  data_extracted['MuseumName'] +'n/' +'<br>Precise Rating: ' + data_extracted['PreciseRating'].astype(str)

# Initialize the Dash app
app = dash.Dash(__name__)

# Define the layout of the app
app.layout = html.Div([
    # Main title    
    html.H1('Interactive Museum Visitor Dashboard'),
    # Dashboard description
    html.P('This dashboard provides insights into museums around the world, showcasing their geographical distribution. Explore the interactive visualizations by clicking on the map marker.'),
    # World map graph    
    dcc.Graph(
        id='world-map',
        figure={
            'data': [
                go.Scattergeo(
                    lon = data_extracted['Langtitude'],
                    lat = data_extracted['Latitude'],
                    text = data_extracted['MuseumName'],
                    hovertext = data_extracted['hover_text'],
                    mode = 'markers',
                    marker = dict(
                        color = data_extracted['PreciseRating'],
                        colorscale = 'Magma',
                        size = 10,
                        colorbar_title = "Precise Rating"
                    )
                )
            ],
            'layout': go.Layout(
                title = 'Museums Around the World',
                titlefont = {"size": 26},
                geo = dict(
                    projection_type="natural earth",
                    showcountries=True, countrycolor="RebeccaPurple"
                )
            )
        }
    ),
    # Placeholder for the bar chart
    dcc.Graph(id='visitor-bar-chart')
])

# Callback function to update the bar chart based on map interaction
@app.callback(
    Output('visitor-bar-chart', 'figure'),
    [Input('world-map', 'clickData')]
)
def update_bar_chart(clickData):
    # Extracting museum name from clickData or defaulting to the first item in the dataset
    museum_name = clickData['points'][0]['text'] if clickData else data_extracted['MuseumName'].iloc[0]
    filtered_df = data_extracted[data_extracted['MuseumName'] == museum_name]

    # Creating the bar chart figure with visitor type counts
    bar_fig = go.Figure(data=[
    go.Bar(name='Families', x=['Visitor Type'], y=[filtered_df['Families_Count'].sum()], text=[filtered_df['Families_Count'].sum()], textfont=dict(size=16), textposition='auto'),
    go.Bar(name='Couples', x=['Visitor Type'], y=[filtered_df['Couples_Count'].sum()], text=[filtered_df['Couples_Count'].sum()], textfont=dict(size=16), textposition='auto'),
    go.Bar(name='Solo', x=['Visitor Type'], y=[filtered_df['Solo_Count'].sum()], text=[filtered_df['Solo_Count'].sum()], textfont=dict(size=16), textposition='auto'),
    go.Bar(name='Business', x=['Visitor Type'], y=[filtered_df['Business_Count'].sum()], text=[filtered_df['Business_Count'].sum()], textfont=dict(size=16), textposition='auto'),
    go.Bar(name='Friends', x=['Visitor Type'], y=[filtered_df['Friends_Count'].sum()], text=[filtered_df['Friends_Count'].sum()], textfont=dict(size=16), textposition='auto')
])

    # Update bar chart properties and returning it
    bar_fig.update_traces(texttemplate='%{text}', textangle=0)
    bar_fig.update_layout(
        title_text='Total Count For Different Type of Traveller',
        titlefont={"size": 26},
        barmode='group',
        legend=dict(orientation="h", yanchor="bottom", y=1.02, xanchor="right", x=1)
    )


    return bar_fig

if __name__ == '__main__':
    app.run_server(debug=True, port=8051)


