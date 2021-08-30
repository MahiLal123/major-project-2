# major-project-2
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "playstore data analysis.ipynb",
      "provenance": [],
      "authorship_tag": "ABX9TyMKprU3JcOvPn2JKHbxIrzt",
      "include_colab_link": true
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/Aaaaaamz/google-app-rating/blob/main/playstore_data_analysis.ipynb\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "C7Xr780UGEqN"
      },
      "source": [
        "**MAJOR PROJECT 2**"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "z-s25xlPF0f1"
      },
      "source": [
        "**Importing required libraries**"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "szEQ9IfeCtFX"
      },
      "source": [
        "import pandas as pd\n",
        "import seaborn as sns\n",
        "import numpy as np\n",
        "import re\n",
        "from scipy.stats import mannwhitneyu\n",
        "from matplotlib import pyplot as plt"
      ],
      "execution_count": 1,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "2Pa0vzPrHdPN"
      },
      "source": [
        "Importing the give data in data frame"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 388
        },
        "id": "_O9HsiFGC-To",
        "outputId": "34282c7f-67ef-4d08-f732-e0c356e0c852"
      },
      "source": [
        "df = pd.read_csv('googleplaystore.csv')\n",
        "df.head(5)"
      ],
      "execution_count": 4,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>App</th>\n",
              "      <th>Category</th>\n",
              "      <th>Rating</th>\n",
              "      <th>Reviews</th>\n",
              "      <th>Size</th>\n",
              "      <th>Installs</th>\n",
              "      <th>Type</th>\n",
              "      <th>Price</th>\n",
              "      <th>Content Rating</th>\n",
              "      <th>Genres</th>\n",
              "      <th>Last Updated</th>\n",
              "      <th>Current Ver</th>\n",
              "      <th>Android Ver</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>Photo Editor &amp; Candy Camera &amp; Grid &amp; ScrapBook</td>\n",
              "      <td>ART_AND_DESIGN</td>\n",
              "      <td>4.1</td>\n",
              "      <td>159</td>\n",
              "      <td>19M</td>\n",
              "      <td>10,000+</td>\n",
              "      <td>Free</td>\n",
              "      <td>0</td>\n",
              "      <td>Everyone</td>\n",
              "      <td>Art &amp; Design</td>\n",
              "      <td>January 7, 2018</td>\n",
              "      <td>1.0.0</td>\n",
              "      <td>4.0.3 and up</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>Coloring book moana</td>\n",
              "      <td>ART_AND_DESIGN</td>\n",
              "      <td>3.9</td>\n",
              "      <td>967</td>\n",
              "      <td>14M</td>\n",
              "      <td>500,000+</td>\n",
              "      <td>Free</td>\n",
              "      <td>0</td>\n",
              "      <td>Everyone</td>\n",
              "      <td>Art &amp; Design;Pretend Play</td>\n",
              "      <td>January 15, 2018</td>\n",
              "      <td>2.0.0</td>\n",
              "      <td>4.0.3 and up</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>U Launcher Lite – FREE Live Cool Themes, Hide ...</td>\n",
              "      <td>ART_AND_DESIGN</td>\n",
              "      <td>4.7</td>\n",
              "      <td>87510</td>\n",
              "      <td>8.7M</td>\n",
              "      <td>5,000,000+</td>\n",
              "      <td>Free</td>\n",
              "      <td>0</td>\n",
              "      <td>Everyone</td>\n",
              "      <td>Art &amp; Design</td>\n",
              "      <td>August 1, 2018</td>\n",
              "      <td>1.2.4</td>\n",
              "      <td>4.0.3 and up</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>Sketch - Draw &amp; Paint</td>\n",
              "      <td>ART_AND_DESIGN</td>\n",
              "      <td>4.5</td>\n",
              "      <td>215644</td>\n",
              "      <td>25M</td>\n",
              "      <td>50,000,000+</td>\n",
              "      <td>Free</td>\n",
              "      <td>0</td>\n",
              "      <td>Teen</td>\n",
              "      <td>Art &amp; Design</td>\n",
              "      <td>June 8, 2018</td>\n",
              "      <td>Varies with device</td>\n",
              "      <td>4.2 and up</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>Pixel Draw - Number Art Coloring Book</td>\n",
              "      <td>ART_AND_DESIGN</td>\n",
              "      <td>4.3</td>\n",
              "      <td>967</td>\n",
              "      <td>2.8M</td>\n",
              "      <td>100,000+</td>\n",
              "      <td>Free</td>\n",
              "      <td>0</td>\n",
              "      <td>Everyone</td>\n",
              "      <td>Art &amp; Design;Creativity</td>\n",
              "      <td>June 20, 2018</td>\n",
              "      <td>1.1</td>\n",
              "      <td>4.4 and up</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "                                                 App  ...   Android Ver\n",
              "0     Photo Editor & Candy Camera & Grid & ScrapBook  ...  4.0.3 and up\n",
              "1                                Coloring book moana  ...  4.0.3 and up\n",
              "2  U Launcher Lite – FREE Live Cool Themes, Hide ...  ...  4.0.3 and up\n",
              "3                              Sketch - Draw & Paint  ...    4.2 and up\n",
              "4              Pixel Draw - Number Art Coloring Book  ...    4.4 and up\n",
              "\n",
              "[5 rows x 13 columns]"
            ]
          },
          "metadata": {},
          "execution_count": 4
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "nEJR9lCrHnum"
      },
      "source": [
        "Checking duplicate data"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "750mN5faDSbl",
        "outputId": "5b244826-3a44-4c26-d2fe-ef0eadf331cf"
      },
      "source": [
        "n_duplicated = df.duplicated(subset=['App']).sum()\n",
        "print(\"There are {}/{} duplicated records.\".format(n_duplicated, df.shape[0]))\n",
        "df_no_dup = df.drop(df.index[df.App.duplicated()], axis=0)\n",
        "print(\"{} records after dropping duplicated.\".format(df_no_dup.shape[0]))"
      ],
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "There are 1181/10841 duplicated records.\n",
            "9660 records after dropping duplicated.\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "B1uIJdHdGcV-"
      },
      "source": [
        "Cleaning type values"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "7RxQnAZlDklt",
        "outputId": "256c2902-6744-408e-af76-e0bdcea7a6e1"
      },
      "source": [
        "print(set(df_no_dup.Type))\n",
        "print(\"Dropping alien Type value '0', {} record(s) removed\".format(sum(df_no_dup.Type == '0')))\n",
        "df_no_dup = df_no_dup.drop(df_no_dup.index[df_no_dup.Type == '0'], axis=0)"
      ],
      "execution_count": 8,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "{nan, '0', 'Paid', 'Free'}\n",
            "Dropping alien Type value '0', 1 record(s) removed\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "_SRamIGxIFfv"
      },
      "source": [
        "checking and droping null values"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "WTuWF8wlDo2M",
        "outputId": "b65b276a-fcdd-4316-c707-81e840d1d8d4"
      },
      "source": [
        "print(\"NaA value statistics in each column\")\n",
        "print(df_no_dup.isnull().sum(axis=0),'\\n')\n",
        "df_no_dup = df_no_dup.dropna(subset=['Type'])\n",
        "print(\"Column 'Type' with NaN values are dropped, {} records left.\".format(df_no_dup.shape[0]))"
      ],
      "execution_count": 25,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "NaA value statistics in each column\n",
            "App                  0\n",
            "Category             0\n",
            "Rating            1462\n",
            "Reviews              0\n",
            "Size                 0\n",
            "Installs             0\n",
            "Type                 0\n",
            "Price                0\n",
            "Content Rating       0\n",
            "Genres               0\n",
            "Last Updated         0\n",
            "Current Ver          8\n",
            "Android Ver          2\n",
            "dtype: int64 \n",
            "\n",
            "Column 'Type' with NaN values are dropped, 9658 records left.\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "EJaowh9pId86"
      },
      "source": [
        "makinng rating dataframe"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "H-hHlVPgIV71",
        "outputId": "cd1c1397-cf43-432c-e94d-4ab2e2d48a65"
      },
      "source": [
        "df_rating = df_no_dup.dropna(subset=['Rating'])\n",
        "print(\"Cleaned dataframe for 'Rating' has {} records.\".format(df_rating.shape[0]))"
      ],
      "execution_count": 23,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Cleaned dataframe for 'Rating' has 8196 records.\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "7X8BWHwZJus8"
      },
      "source": [
        "here i am analysing rating,paidfree,type so only adding them in dataframe"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "YLgy2UyaDu3u"
      },
      "source": [
        "df_rating = df_rating.loc[:,['Rating', 'Type', 'Category']]"
      ],
      "execution_count": 26,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "UrRxdLhfI7le"
      },
      "source": [
        "Definig histogram"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "3AZBu8TEI7Ms"
      },
      "source": [
        "def plot_hist(df, col, bins=10):\n",
        "    \"\"\"\n",
        "    Plot histograms for a column\n",
        "    \"\"\"\n",
        "    plt.hist(df[col], bins=bins)\n",
        "    plt.xlabel(col)\n",
        "    plt.ylabel('counts')\n",
        "    plt.title('Distribution of {}'.format(col))"
      ],
      "execution_count": 27,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "WSsUbIfuJDOS"
      },
      "source": [
        "arranging no of paid and free apps"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "MJ-LvCa8I8J1"
      },
      "source": [
        "def compute_app_types(df):\n",
        "    \n",
        "    return sum(df.Type == \"Free\"), sum(df.Type == 'Paid')"
      ],
      "execution_count": 28,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "Q3u1V969I6p1"
      },
      "source": [
        "adding the h paod free app values to axis of graphs"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "LT6CLrjWJSc9"
      },
      "source": [
        "def plot_app_types(df):\n",
        "    \n",
        "    vc_rating = df.Category.value_counts()\n",
        "    cat_free_apps = []\n",
        "    cat_paid_apps = []\n",
        "    for cat in vc_rating.index:\n",
        "        n_free, n_paid = compute_app_types(df.query(\"Category == '{}'\".format(cat)))\n",
        "        cat_free_apps.append(n_free)\n",
        "        cat_paid_apps.append(n_paid)\n",
        "\n",
        "    f, ax = plt.subplots(2,1)\n",
        "    ax[0].bar(range(1, len(cat_free_apps)+1), cat_free_apps)\n",
        "    ax[1].bar(range(1, len(cat_free_apps)+1), cat_paid_apps)\n"
      ],
      "execution_count": 29,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "yGorKqP1JOhH"
      },
      "source": [
        "plotting graph"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 574
        },
        "id": "q7urL5LSEB3t",
        "outputId": "14ab945a-4468-41b2-c87a-d6199212b73c"
      },
      "source": [
        "plot_hist(df_rating, 'Rating')\n",
        "df_rating.describe()"
      ],
      "execution_count": 30,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>Rating</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>count</th>\n",
              "      <td>8196.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>mean</th>\n",
              "      <td>4.173243</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>std</th>\n",
              "      <td>0.536625</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>min</th>\n",
              "      <td>1.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>25%</th>\n",
              "      <td>4.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>50%</th>\n",
              "      <td>4.300000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>75%</th>\n",
              "      <td>4.500000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>max</th>\n",
              "      <td>5.000000</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "            Rating\n",
              "count  8196.000000\n",
              "mean      4.173243\n",
              "std       0.536625\n",
              "min       1.000000\n",
              "25%       4.000000\n",
              "50%       4.300000\n",
              "75%       4.500000\n",
              "max       5.000000"
            ]
          },
          "metadata": {},
          "execution_count": 30
        },
        {
          "output_type": "display_data",
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          }
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "DxAUrHrOEEbM",
        "outputId": "4f1a9539-8a2c-4595-91cb-ade03e783445"
      },
      "source": [
        "print(\"There are {} free and {} paid apps in the the Rating dataframe \".format(*compute_app_types(df_rating)))"
      ],
      "execution_count": 13,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "There are 7592 free and 604 paid apps in the the Rating dataframe \n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "eACAY9ANKnuN"
      },
      "source": [
        "Rating distribution of paid and free apps"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 268
        },
        "id": "JCf5dHCHER29",
        "outputId": "119b1c00-3ecf-46e1-ddca-0905d152ba66"
      },
      "source": [
        "\n",
        "plot_app_types(df_rating)"
      ],
      "execution_count": 14,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 2 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          }
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 295
        },
        "id": "df0Av_ggEbVG",
        "outputId": "4110978b-df50-4903-c930-f13dafb0147a"
      },
      "source": [
        "df_rating.describe()"
      ],
      "execution_count": 16,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>Rating</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>count</th>\n",
              "      <td>5753.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>mean</th>\n",
              "      <td>4.173197</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>std</th>\n",
              "      <td>0.544844</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>min</th>\n",
              "      <td>1.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>25%</th>\n",
              "      <td>4.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>50%</th>\n",
              "      <td>4.300000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>75%</th>\n",
              "      <td>4.500000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>max</th>\n",
              "      <td>5.000000</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "            Rating\n",
              "count  5753.000000\n",
              "mean      4.173197\n",
              "std       0.544844\n",
              "min       1.000000\n",
              "25%       4.000000\n",
              "50%       4.300000\n",
              "75%       4.500000\n",
              "max       5.000000"
            ]
          },
          "metadata": {},
          "execution_count": 16
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "rlsMs4PoK0-h"
      },
      "source": [
        "plotting the average rating of paid and free apps"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 346
        },
        "id": "V1YTCryjEjhd",
        "outputId": "dc5f8469-c60d-42b6-a0f1-95d4c89b2d70"
      },
      "source": [
        "def plot_target_by_group(df, target_col, group_col, figsize=(6,4), title=\"\"):\n",
        "    \n",
        "    order = sorted(list(set(df[group_col])))\n",
        "    stats = df.groupby(group_col).mean()[target_col]\n",
        "    fig, ax = plt.subplots(figsize=figsize)\n",
        "    sns.barplot(x=group_col, y=target_col, data=df, ax=ax, order=order).set_title(title)\n",
        "    ax.set(ylim=(3.8, 4.5))    \n",
        "    return stats\n",
        "    \n",
        "stats = plot_target_by_group(df_rating, 'Rating', 'Type', title=\"Average Rating Groupped by App Type\")\n",
        "for i, s in zip(stats.index, stats):\n",
        "    print(\"{} app has average {} {}\".format(i, 'Rating',s))\n",
        "mean_rating = df_rating.Rating.mean()\n",
        "print(\"Mean rating: {}\".format(mean_rating))"
      ],
      "execution_count": 31,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Free app has average Rating 4.1663724973656535\n",
            "Paid app has average Rating 4.259602649006619\n",
            "Mean rating: 4.173243045387998\n"
          ],
          "name": "stdout"
        },
        {
          "output_type": "display_data",
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          }
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "JdzAg2DPMLYo"
      },
      "source": [
        "plotting the difference between rating of paid and free apps"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 518
        },
        "id": "4Z3WlZeSEzf6",
        "outputId": "2b3d4085-d7f9-426f-ea74-c55497d1b2a9"
      },
      "source": [
        "fig, ax = plt.subplots(figsize=(16,4))\n",
        "sorted_idx = sorted(paid_stats.index)\n",
        "rating_diff = paid_stats[sorted_idx] - free_stats[sorted_idx]\n",
        "sns.barplot(x=sorted_idx, y=rating_diff, ax=ax).set_title(\"Difference of Ratings between Paid and Free Apps Across App Categories\");\n",
        "rating_diff"
      ],
      "execution_count": 20,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Category\n",
              "COMMUNICATION        -0.063287\n",
              "FAMILY                0.115128\n",
              "FINANCE              -0.297604\n",
              "GAME                  0.136364\n",
              "HEALTH_AND_FITNESS    0.154858\n",
              "LIFESTYLE             0.166608\n",
              "MEDICAL               0.087344\n",
              "PERSONALIZATION       0.147692\n",
              "PHOTOGRAPHY          -0.152796\n",
              "PRODUCTIVITY          0.023577\n",
              "SPORTS                0.041940\n",
              "TOOLS                 0.142818\n",
              "Name: Rating, dtype: float64"
            ]
          },
          "metadata": {},
          "execution_count": 20
        },
        {
          "output_type": "display_data",
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 1152x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          }
        }
      ]
    }
  ]
}
