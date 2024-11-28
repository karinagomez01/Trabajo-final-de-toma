# Trabajo-final-de-toma
import pandas as pd
import  numpy  as  np 
import  matplotlib.pyplot  as  plt 
import  seaborn  as  sns 
import  sqlite3 
file_name = 'data.csv'
df = pd.read_csv(file_name)
print(df.head())
db_name = 'data.sqlite'
conn = sqlite3.connect(db_name)

table_name = 'data'
df.to_sql(table_name, conn, if_exists='replace', index=False)

query = f"SELECT * FROM {table_name} LIMIT 5"
df_from_sql = pd.read_sql(query, conn)
print(df_from_sql)

conn.close()
data = pd.read_csv("data.csv")
print("Descripción estadística básica:")
print(data.describe(include='all'))

print("\nValores faltantes por columna:")
print(data.isnull().sum())

sleep_mapping = {
    "Less than 5 hours": 4,
    "5-6 hours": 5.5,
    "7-8 hours": 7.5,
    "More than 8 hours": 9
}
data["Sleep Duration (numeric)"] = data["Sleep Duration"].map(sleep_mapping)
# 1. Gráfico de barras: Género vs. Depresión
plt.figure(figsize=(8, 6))
sns.countplot(x="Gender", hue="Depression", data=data)
plt.title("Distribución de Depresión por Género")
plt.xlabel("Género")
plt.ylabel("Cantidad")
plt.legend(title="Depresión")
plt.show()
# 2. Gráfico de dispersión: Horas de estudio vs. Presión académica
plt.figure(figsize=(8, 6))
sns.scatterplot(x="Study Hours", y="Academic Pressure", hue="Depression", data=data)
plt.title("Horas de Estudio vs. Presión Académica")
plt.xlabel("Horas de Estudio")
plt.ylabel("Presión Académica")
plt.legend(title="Depresión")
plt.show()
# 3. Gráfico de torta: Distribución de hábitos alimenticios
plt.figure(figsize=(8, 6))
data["Dietary Habits"].value_counts().plot.pie(autopct="%1.1f%%", startangle=90, colors=sns.color_palette("pastel"))
plt.title("Distribución de Hábitos Alimenticios")
plt.ylabel("")  # Quitar etiqueta del eje Y
plt.show()
