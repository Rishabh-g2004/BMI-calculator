import tkinter as tk
from tkinter import messagebox, font

class BMICalculator(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("BMI Calculator with Diet Plan")
        self.geometry("640x540")
        self.configure(bg="#4a7fff")
        self.resizable(False, False)

        # Setting fonts
        self.default_font = font.Font(family="Segoe UI", size=10)
        self.title_font = font.Font(family="Segoe UI", size=14, weight="bold")
        self.label_font = font.Font(family="Segoe UI", size=12, weight="bold")
        self.message_font = font.Font(family="Segoe UI", size=10, slant="italic")

        self.diet_preference = tk.StringVar(value="Vegetarian")
        self.create_widgets()

    def create_widgets(self):
        title_label = tk.Label(self, text="BMI Calculator with Diet Plan", font=self.title_font, bg="#4a7fff", fg="white")
        title_label.pack(pady=(15, 10))

        weight_label = tk.Label(self, text="Weight (kg):", font=self.label_font, bg="#4a7fff", fg="white")
        weight_label.pack(anchor="w", padx=30)
        self.weight_entry = tk.Entry(self, font=self.default_font, width=25)
        self.weight_entry.pack(padx=30, pady=(0, 10))

        height_label = tk.Label(self, text="Height (cm):", font=self.label_font, bg="#4a7fff", fg="white")
        height_label.pack(anchor="w", padx=30)
        self.height_entry = tk.Entry(self, font=self.default_font, width=25)
        self.height_entry.pack(padx=30, pady=(0, 10))

        # Diet Preference selection
        preference_frame = tk.LabelFrame(self, text="Diet Preference", bg="#4a7fff", fg="white", font=self.label_font, padx=10, pady=5)
        preference_frame.pack(padx=30, pady=10, fill="x")

        veg_radio = tk.Radiobutton(preference_frame, text="Vegetarian", variable=self.diet_preference, value="Vegetarian",
                                   bg="#4a7fff", fg="white", selectcolor="#3366ff", font=self.default_font)
        veg_radio.pack(side="left", padx=10, pady=4)

        nonveg_radio = tk.Radiobutton(preference_frame, text="Non-Vegetarian", variable=self.diet_preference, value="Non-Vegetarian",
                                     bg="#4a7fff", fg="white", selectcolor="#3366ff", font=self.default_font)
        nonveg_radio.pack(side="left", padx=10, pady=4)

        calc_button = tk.Button(self, text="Calculate BMI", font=self.label_font,
                                bg="white", fg="#4a7fff",
                                activebackground="#3366ff", activeforeground="white",
                                command=self.calculate_bmi)
        calc_button.pack(pady=(10, 15))

        self.bmi_result_label = tk.Label(self, text="", font=self.label_font, bg="#4a7fff", fg="white")
        self.bmi_result_label.pack()

        self.bmi_status_label = tk.Label(self, text="", font=self.label_font, bg="#4a7fff", fg="#ffde59")
        self.bmi_status_label.pack(pady=(5, 0))

        self.diet_plan_label = tk.Label(self, text="", font=self.message_font, bg="#4a7fff", fg="white",
                                       wraplength=380, justify="center")
        self.diet_plan_label.pack(padx=20, pady=(5, 20))

    def calculate_bmi(self):
        try:
            weight = float(self.weight_entry.get())
            height_cm = float(self.height_entry.get())
            if weight <= 0 or height_cm <= 0:
                raise ValueError
        except ValueError:
            messagebox.showerror("Invalid input", "Please enter valid positive numbers for weight and height.")
            return

        height_m = height_cm / 100
        bmi = weight / (height_m * height_m)
        bmi_rounded = round(bmi, 1)

        self.bmi_result_label.config(text=f"Your BMI is {bmi_rounded}")

        self.bmi_status_label.config(text="")
        self.diet_plan_label.config(text="")

        pref = self.diet_preference.get()

        # Set status and diet plan based on BMI and diet preference
        if bmi <= 18.5:
            self.bmi_status_label.config(text="You have lower weight than normal body!")
            if pref == "Vegetarian":
                diet_text = ("Diet Plan (Vegetarian): Increase calorie intake with nutrient-dense vegetarian foods such as nuts, seeds, dairy, legumes, whole grains, and healthy oils. "
                             "Consider frequent nutritious meals and snacks.")
            else:
                diet_text = ("Diet Plan (Non-Vegetarian): Increase calorie intake including lean meats, fish, eggs, dairy, nuts, and whole grains. "
                             "Include frequent nutrient-rich meals and snacks.")
            self.diet_plan_label.config(text=diet_text)

        elif 18.5 < bmi <= 25:
            self.bmi_status_label.config(text="Normal!")
            if pref == "Vegetarian":
                diet_text = ("Diet Plan (Vegetarian): Maintain a balanced diet rich in vegetables, fruits, legumes, nuts, dairy, and whole grains to stay healthy.")
            else:
                diet_text = ("Diet Plan (Non-Vegetarian): Maintain a balanced diet with vegetables, fruits, lean meats, fish, eggs, dairy, and whole grains.")
            self.diet_plan_label.config(text=diet_text)

        elif 25 < bmi <= 30:
            self.bmi_status_label.config(text="Overweight!")
            if pref == "Vegetarian":
                diet_text = ("Diet Plan (Vegetarian): Reduce intake of sugars and saturated fats. Focus on portion control, increase fiber-rich foods like vegetables, fruits, legumes, and whole grains. Stay active regularly.")
            else:
                diet_text = ("Diet Plan (Non-Vegetarian): Reduce sugars and saturated fats. Control portions, increase fiber-rich foods such as vegetables, fruits, whole grains, and lean proteins. Maintain regular physical activity.")
            self.diet_plan_label.config(text=diet_text)

        else:
            self.bmi_status_label.config(text="Obese!")
            if pref == "Vegetarian":
                diet_text = ("Diet Plan (Vegetarian): Consult a healthcare professional. Adopt a calorie-controlled vegetarian diet focusing on vegetables, fruits, whole grains, and legumes. Increase physical activity and focus on long-term lifestyle changes.")
            else:
                diet_text = ("Diet Plan (Non-Vegetarian): Consult a healthcare professional. Follow a calorie-controlled diet focusing on lean proteins, vegetables, fruits, and whole grains. Increase physical activity and aim for sustainable lifestyle changes.")
            self.diet_plan_label.config(text=diet_text)

if __name__ == "__main__":
    app = BMICalculator()
    app.mainloop()

