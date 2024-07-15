# shivpratap.taskcodealpha
#1 code for hangman
import random 
from collections import Counter 

someWords = '''apple banana mango strawberry orange '''
someWords = someWords.split(' ') 
word = random.choice(someWords) 

if __name__ == '__main__': 
	print('Guess the word! HINT: word is a name of a fruit') 

	for i in word: 
		print('_', end=' ') 
	print() 

	playing = True
	letterGuessed = '' 
	chances = len(word) + 2
	correct = 0
	flag = 0
	try: 
		while (chances != 0) and flag == 0: 
			print() 
			chances -= 1

			try: 
				guess = str(input('Enter a letter to guess: ')) 
			except: 
				print('Enter only a letter!') 
				continue 
			if not guess.isalpha(): 
				print('Enter only a LETTER') 
				continue
			elif len(guess) & gt > 1: 
				print('Enter only a SINGLE letter') 
				continue
			elif guess in letterGuessed: 
				print('You have already guessed that letter') 
				continue 
			if guess in word:  
				k = word.count(guess) 
				for _ in range(k): 
					letterGuessed += guess
			for char in word: 
				if char in letterGuessed and (Counter(letterGuessed) != Counter(word)): 
					print(char, end=' ') 
					correct += 1
				elif (Counter(letterGuessed) == Counter(word)): 
					print("The word is: ", end=' ') 
					print(word) 
					flag = 1
					print('Congratulations, You won!') 
					break  
					break  
				else: 
					print('_', end=' ')  
		if chances & lt<= 0 and (Counter(letterGuessed) != Counter(word)): 
			print() 
			print('You lost! Try again..') 
			print('The word was {}'.format(word)) 

	except KeyboardInterrupt: 
		print() 
		print('Bye! Try again.') 
		exit() 
#2 code for stock portfolio tracker
import yfinance  as yf

portfolio = {}

def show_menu():
    print("\n1. View Portfolio")
    print("2. Add Stock")
    print("3. Remove Stock")
    print("4. View Performance")
    print("5. Exit")

def view_portfolio():
    if not portfolio:
        print("\nNo stocks in the portfolio.")
    else:
        print("\nPortfolio:")
        for ticker, shares in portfolio.items():
            print(f"{ticker}: {shares} shares")

def add_stock():
    ticker = input("Enter the stock ticker: ").upper()
    shares = int(input(f"Enter the number of shares for {ticker}: "))
    portfolio[ticker] = portfolio.get(ticker, 0) + shares
    print(f"Added {shares} shares of {ticker}.")

def remove_stock():
    ticker = input("Enter the stock ticker to remove: ").upper()
    if ticker in portfolio:
        shares = int(input(f"Enter the number of shares to remove for {ticker}: "))
        if shares >= portfolio[ticker]:
            del portfolio[ticker]
            print(f"Removed all shares of {ticker}.")
        else:
            portfolio[ticker] -= shares
            print(f"Removed {shares} shares of {ticker}.")
    else:
        print(f"{ticker} is not in the portfolio.")

def view_performance():
    if not portfolio:
        print("\nNo stocks in the portfolio to track performance.")
    else:
        total_value = 0
        print("\nPortfolio Performance:")
        for ticker, shares in portfolio.items():
            stock = yf.Ticker(ticker)
            price = stock.history(period="1d")['Close'][0]
            value = price * shares
            total_value += value
            print(f"{ticker}: {shares} shares @ ${price:.2f} each, Total Value: ${value:.2f}")
        print(f"Total Portfolio Value: ${total_value:.2f}")

def main():
    while True:
        show_menu()
        choice = input("Choose an option (1-5): ")
        if choice == '1':
            view_portfolio()
        elif choice == '2':
            add_stock()
        elif choice == '3':
            remove_stock()
        elif choice == '4':
            view_performance()
        elif choice == '5':
            print("Exiting.")
            break
        else:
            print("Invalid choice. Please enter a number between 1 and 5.")

#3 code for task automation with python script
import os
import shutil
import pandas as pd

def organize_files(source_dir, dest_dir):
    if not os.path.exists(dest_dir):
        os.makedirs(dest_dir)
    for filename in os.listdir(source_dir):
        if filename.endswith('.csv'):
            shutil.move(os.path.join(source_dir, filename), os.path.join(dest_dir, filename))
    print("Files organized.")


def clean_data(file_path):
    df = pd.read_csv(file_path)
    df.fillna(0, inplace=True)  
    cleaned_file_path = file_path.replace('.csv', '_cleaned.csv')
    df.to_csv(cleaned_file_path, index=False)
    print(f"Data cleaned and saved to {cleaned_file_path}.")
    return cleaned_file_path


def generate_report(cleaned_file_path):
    df = pd.read_csv(cleaned_file_path)
    report = df.describe()
    report_file_path = cleaned_file_path.replace('_cleaned.csv', '_report.csv')
    report.to_csv(report_file_path)
    print(f"Report generated and saved to {report_file_path}.")

def delete_temp_files(temp_dir):
    if os.path.exists(temp_dir):
        shutil.rmtree(temp_dir)
        print(f"Temporary files in {temp_dir} deleted.")
    else:
        print(f"No temporary files found in {temp_dir}.")

if __name__ == "__main__":
    source_directory = 'source_files'
    organized_directory = 'organized_files'
    data_file = 'organized_files/sample.csv'  
    temp_directory = 'temp_files'

    organize_files(source_directory, organized_directory)
    cleaned_data_file = clean_data(data_file)
    generate_report(cleaned_data_file)
    delete_temp_files(temp_directory)
