func writeToFile(rowKeyList []string) error {
	if len(rowKeyList) == 0 {
		return nil
	}

	file, err := os.OpenFile(fileName, os.O_CREATE | os.O_RDWR, 0666)
	if err != nil {
		return err
	}
	defer file.Close()

	file.Truncate(0)
	for _, str := range rowKeyList {
		file.WriteString(str + "\n")
	}

	return nil
}

func readFile() ([]string, error) {
	file, err := os.OpenFile(fileName, os.O_RDONLY, 0666)
	if err != nil {
		return nil, err
	}
	defer file.Close()

	br := bufio.NewReader(file)
	rowKeyList := make([]string, 0)
	for {
		line, _, err := br.ReadLine()
		if err != nil {
			if err == io.EOF {
				return rowKeyList, nil
			}
			return rowKeyList, err
		}

		rowKeyList = append(rowKeyList, string(line))
	}
}